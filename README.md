# Zachran Jidlo & DODO automation
**"Zachran jidlo" is a czech charitable organization which is taking care of meal distribution from restaurants to the charities.**

These scripts handles automated orders creation using DODO delivery service public API based on data saved in private Zachran Jidlo's Airtable.

There are two main scripts.
### Send orders
Can be executed with command `npm run start:send`.

- Loads donors (table `Dárci`) and charities (table `Příjemci`) from airtable
- Creates order on DODO for every donor and charity for next week (+7 days)
- Saves info about the order to airtable (table `Rozvozy`)

Unique key for every order is created from pattern `{donor}-{charity}-d.m.yyyy` example: `test-restaurace-1-charita13-6.10.2022`. This prevents creating duplicates. Script is executed every weekday using github actions's cron.

### Check orders
Can be executed with command `npm run start:check`.

- Loads unconfirmed orders (table `Rozvozy`) with status waiting for current day
- Checks `Nabídka` table if there is a confirmation for the order
  - If yes, confirms the order (status is set to confirmed)
  - If no, checks if there is still a time for confirmation and if not, cancel the order

Script is executed every 15 minutes on weekdays using github actions's cron.

## Development
Nodejs 16 is required. Typescript is transformed by [ts-node](https://www.npmjs.com/package/ts-node) library.

There are some secrets required. You can create `.env` file in the root folder.
```
AIRTABLE_API_KEY="key" // You can find this key in your Airtable profile
DODO_CLIENT_ID="client" // Ask someone from DODO/Zachran jidlo for this one
DODO_CLIENT_SECRET="secret" // Ask someone from DODO/Zachran jidlo for this one
DODO_SCOPE="scope" // Ask someone from DODO/Zachran jidlo for this one
DODO_OAUTH_URI="oauth" // Ask someone from DODO/Zachran jidlo for this one
DODO_ORDERS_API="api" // Ask someone from DODO/Zachran jidlo for this one
```
