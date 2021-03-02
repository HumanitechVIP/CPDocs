---
layout: default
title: How it Works
parent: API
nav_order: 1
---

By Vincent Huynh

# How It Works

## File Structure

```dotnetcli
.
├── config
│   ├── db.js
│   ├── default.json
│   └── readme.md
├── middleware
│   └── auth.js
├── models
│   ├── Pricing.js
│   └── User.js
├── package.json
├── package-lock.json
├── README.md
├── routes
│   └── api
│       ├── auth.js
│       ├── cropprices.js
│       └── users.js
└── server.js
```

The above is the file structure for the backend. Here is how it works.

## server.js

`server.js` is the starting point for the program.

1. The backend API uses ExpressJS to handle all requests
1. `server.js` calls `connectDB()`, which just initializes the DB as outlined in `config/db.js`
1. `server.js` adds all of the relevant routes:

```javascript
app.get("/", (req, res) => res.send("You've reached the Crop Pricing API!!!"));
app.use("/api/cropprices", require("./routes/api/cropprices"));
app.use("/api/users", require("./routes/api/users"));
app.use("/api/auth", require("./routes/api/auth"));
```

4. `server.js` sets the port it runs on to either `process.env.PORT` or `5000` if the former is not defined

## routes/api/\*

All files in `routes/api` handle the specific HTTP requests
For more detailed information, please refer to the content in the next few tabs (on the sidebar).

Essentially,

### cropprices.js

Handles entry of crop prices using POST requests. Breaking down one of the codeblocks may help you understand the rest of the code:

```javascript
router.post(
  "/",
  [
    check("code", "code required").exists(),
    check("unit", "Unit required").exists(),
    check("weight", "Weight required").exists(),
    check("price", "Price required").exists(),
    check("variety", "Variety required").exists(),
    check("market", "Market required").exists(),
  ],
  async (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
```

The above block is input validation. It just makes sure that all of the required data according to the MondoDB schema is met. If it is not met, a JSON containing an array titled "errors" will be returned to the caller with all the relevant error messages, such as "code required."

```javascript
    try {
      let pricingListing = await Pricing.findOne({ code });
      if (pricingListing) {
        return res
          .status(400)
          .json({ errors: [{ msg: "Listing already exists " }] });
      }
```

The above block checks if a listing with the provided code already exists; if so, a JSON object with an array titled "errors" will be returned stating that a listing already exists.

```javascript
pricing = new Pricing({
  code,
  unit,
  weight,
  price,
  variety,
  market,
});

if (average) pricing.average = average;
if (max) pricing.max = max;
if (min) pricing.min = min;
if (stdev) pricing.stdev = stdev;

await pricing.save();

const payload = {
  pricing: {
    id: pricing.id,
  },
};
res.json(pricing);
```

The above block creates a new Pricing listing based on the schema outlined in `models/Pricing`. The required fields are automatically populated. Optional field are checked, then populated in the object. The listing is then posted with `await pricing.save()`, which then asynchronously saves the object to the MongoDB database. A `payload` object is then created (this will be used in the frontend, but for now is unused). The `pricing` object is now returned.

```javascript
    } catch (err) {
      console.error(err);
      res.status(500).send("Server Error");
    }
  }
);
```

The above block just catches any errors and gives a generic error. This should probably be more detailed, but this is more of a catch-all error.

## Models

The `models` directory outlines the schema for the mongodb database. Here is an example:

```javascript
const mongoose = require("mongoose");

const PricingSchema = new mongoose.Schema({
  code: {
    type: String,
    required: true,
  },
  unit: {
    type: String,
    required: true,
  },
  weight: {
    type: String,
    required: true,
  },
  price: {
    type: Number,
    required: true,
  },
  variety: {
    type: String,
    required: true,
  },
  market: {
    type: String,
    required: true,
  },
  average: {
    type: Number,
  },
  max: {
    type: Number,
  },
  min: {
    type: Number,
  },
  stdev: {
    type: Number,
  },
  date: {
    type: Date,
    default: Date.now,
  },
});

module.exports = Pricing = mongoose.model("pricing", PricingSchema);
```

The schema is populated with all relevant and important information, including the name, type, and if the information is required. Also, the date. At the end, this is exported for use in other areas of the code, as shown above.

### MongoDB Atlas

At the moment, the MongoDB Atlas share is under `humanitechapp@gmail.com` account. You shouldn't need access to this email. I have uploaded the relevant MongoURI (located in `config/default,json`) to the Google Drive in "Super Secret Stuff."
