[<<Back](../README.md)
### server.js 

```javascript
// Import necessary modules and dependencies

// Load environment variables from .env file
dotenv.config();

// Set up Express app
const app = express();

// Define port and hostname
const port = process.env.PORT || 3002;
const hostname = process.env.HOST || "localhost";

// Establish database connection
connectDB();

// Set up CORS middleware
app.use(cors());

// Set up Morgan for HTTP request and response logging
app.use(morgan(process.env.LOGGING_FORMAT || "dev"));

// Set view engine and view path for rendering views
app.set("view engine", "ejs");
app.set("views", "./src/views");

// Set up Express session for handling user sessions
app.use(
  session({
    resave: false, // don't save session if unmodified
    saveUninitialized: false, // don't create session until something stored
    secret: process.env.SESSION_SECRET || "dantisecret", // secret used to sign the session ID cookie
  })
);

// Route for rendering index page
app.get("/", (req, res) => {
  res.render("index");
});

// Middleware to handle JSON data
app.use(bodyParser.json());

// Middleware to parse incoming JSON and urlencoded data
app.use(express.urlencoded({ extended: false }));

// Set up Swagger API documentation
const specs = swaggerJsdoc(swagger);
app.use(
  "/api/documentation",
  swaggerUi.serve,
  swaggerUi.setup(specs, { explorer: true })
);

// Configure routes
configureRoutes(app);

// Serve static files from specified folders
app.use("/assets/uploads", express.static("assets/uploads"));
app.use("/public", express.static("public"));

// Schedule cron jobs for periodic tasks
cron.schedule("0 0 * * *", () => {
});

// Start the Express server
app.listen(port, hostname, () => {
  console.log(`Server is running at http://${hostname}:${port}/`);
});

// Export the Express app for external use
export default app;
