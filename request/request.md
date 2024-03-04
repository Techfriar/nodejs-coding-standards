[<<Back](../README.md)
### example request validation using JOI
```javascript
//custom error messages
const options = {
  messages: joiCustomMessages,
};

export default class AddBankAccountRequest {
  static schema = Joi.object({
    name: Joi.string().required(),
    account_id: Joi.string(),
    currency: Joi.number().required(),
    balance: Joi.number().required(),
    is_confidential: Joi.boolean(),
  }).options(options);

  constructor(data) {
    this.data = data;
  }

  async validate() {
    // Validate the provided 'bankAccount' object against the defined schema
    const { error, value } = AddBankAccountRequest.schema.validate(this.data, {
      abortEarly: false,
    });

    // If there are validation errors, populate the 'validationErrors' object
    // Additional validations for specific properties could be added here

    if (error) {
      const validationErrors = {};
      error?.details.forEach((err) => {
        validationErrors[err.context.key] = cleanErrorMessage(err.message);
      });

      // Throw the transformed validation errors
      throw new CustomValidationError(validationErrors);
    }

    // If validation passes, return the validated bank account object
    return value;
  }
}