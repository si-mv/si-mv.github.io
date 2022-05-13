# Sequelize
Sequelize is an ORM. It makes it easier to interact with our database from within javascript. (Writing raw SQL strings in JS is not fun.)


## Getting set up
In the terminal, you should
```js
npm init
```
and
```js
npm install sqlite3 sequelize
```

Next, make two new files:
 - `db.sqlite` is where the data will be stored
 - `db.js` is where we will set everything up

In `db.js` we will make an instance of sequelize:
```js
const { Sequelize, DataTypes, Model } = require("sequelize");
const sequelize = new Sequelize({
  dialect: "sqlite",
  storage: "db.sqlite",
});
module.exports = { sequelize, DataTypes, Model };
```
We will need `DataTypes` and `Model` in the next part.

Now that sequelize is connected to our storage, let's make a model so we can input data.


## Models
 - A **model** is like a set of blueprints for creating new rows in our databases.
 - When we make a new model, we specify the *type* of each field
 - This is important so bad data doesn't enter the db
 - Using `DataTypes` from sequelize means we don't need to remember the types for each flavour of sql (mysql, postgres, etc)

In a new file called `restaurant.js`, let's make a restaurant model
```js
const { sequelize, DataTypes, Model } = require("./db");
 
class Restaurant extends Model {}
 
Restaurant.init(
  {
    // Model attributes are defined here
    name: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    imageURL: {
      type: DataTypes.STRING,
      allowNull: false,
    },
  },
  {
    // Other model options go here
    sequelize, // We need to pass the connection instance
  }
);
 
module.exports = Restaurant;
```

Now we can use this model `Restaurant` to add data to the database. We are going to write test files to add data and then check that it exists. This means we don't need to manually add and then `console.log` to check that it worked.


## Testing
Testing with jest means we avoid `console.log`ing and getting messed up with test data and real data. Let's see how it works!


Make a file called `db.test.js` and put this at the top:

```js
const { beforeAll, describe, test, expect } = require("@jest/globals");
const { sequelize } = require("./db");
const Restaurant = require("./restaurant");
```
This is everything we need to test the `Restaurant` model.

Next, in this file, we will start a `describe` block for testing:
```js
describe("Restaurant", () => {
  beforeAll(async () => {
    await sequelize.sync({ force: true });
  });
  //...we'll continue here on the next slide
});
```
The `beforeAll` happens before any other test is run. `sequelize.sync()` will ensure data from old test runs doesn't interfere with our current test.

Now, inside the `describe` underneath `beforeAll`, we make our first test:
```js
  test("Can create a restaurant", async () => {

    await Restaurant.create({
      name: "Nando's",
      imageURL:
        "https://wembleypark.com/media/images/Nandos_Platter_in_London_Designer_.2e16d0ba.fill-496x279.png",
    });
    //...we'll continue here on the next slide
```
We have created a new row for Nando's! Let's see if it worked.

Under `Restaurant.create` but still inside the `test`, let's add
```js
const restaurant = await Restaurant.findOne({
    where: {
    name: "Nando's",
    },
});
// ...we'll continue here on the next slide
```
This queries the database to see if it can find the row we added by name. Then...

We ask jest to make sure the data is all present and correct :D
```js
expect(restaurant.id).toBe(1);
expect(restaurant.name).toBe("Nando's");
expect(restaurant.imageURL).toBe(
    "https://wembleypark.com/media/images/Nandos_Platter_in_London_Designer_.2e16d0ba.fill-496x279.png"
);
```

Here's the whole file:
```js
const { beforeAll, describe, test, expect } = require("@jest/globals");
const { sequelize } = require("./db");
const Restaurant = require("./restaurant");
describe("Restaurant", () => {
  beforeAll(async () => {
    await sequelize.sync({ force: true });
  });
  test("Can create a restaurant", async () => {
    await Restaurant.create({
      name: "Nando's",
      imageURL:
        "https://wembleypark.com/media/images/Nandos_Platter_in_London_Designer_.2e16d0ba.fill-496x279.png",
    });
    const restaurant = await Restaurant.findOne({
      where: {
        name: "Nando's",
      },
    });
    expect(restaurant.id).toBe(1);
    expect(restaurant.name).toBe("Nando's");
    expect(restaurant.imageURL).toBe(
      "https://wembleypark.com/media/images/Nandos_Platter_in_London_Designer_.2e16d0ba.fill-496x279.png"
    );
  });
});
```


## That's it!
Try and get this code working, and then add more models and tests to extend the functionality to build out your restaurant app :)

Happy coding
```
（＾∀＾●）ﾉｼ
```