[<<Back](../README.md)

We create two response classes for each entity: one to send the complete content and the other to send minimal content.
### contact response
```javascript
export default async function contactResponse(contact) {
  return {
    id: contact.key_id,
    first_name: contact.first_name,
    last_name: contact.last_name,
    phone: contact.phone,
    country_code: contact.country_code,
    country_unicode: contact.country_unicode,
    email: contact.email,
    company: contact.company ? await companyResponse(contact.company) : null,
    profile_picture: generateFilesUrl(contact.profile_picture),
  };
}
```

### contact min resource
```javascript
export default async function contactResponse(contact) {
  return {
    id: contact.key_id,
    first_name: contact.first_name,
    last_name: contact.last_name,
    phone: contact.phone,
    email: contact.email,
  };
}
```