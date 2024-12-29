# Lazy Resy

I'm hungry and like to eat well. What can I say? 🤷‍♂️

This script allows you to make a reservation at a restaurant on Resy. It's ideally run on a cron/task scheduler job, but can be run
manually as well. Imagine picking your day and ideal time, and then letting the script do the rest. It's that easy. No
more wait-lists, no more checking the app every 5 minutes. Just set it and forget it.

Original credit goes to Rob Dominguez and this repo: https://github.com/robertjdominguez/ez-resy

## Motivation

For a while, COQODAQ was a super hot restaurant in NYC and it was impossible to get a reservation without some automation.

## Installation

Clone the repository:

```bash
git clone https://github.com/atamanch/ez-resy.git
```

Install the dependencies:

```bash
npm i
```

https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

## Configuration

You'll need a `.env` file that contains the below, see the example .env file in the root of the repo:

```env
VENUE_ID=
DATE=
EARLIEST=
LATEST=
PARTY_SIZE=
PAYMENT_ID=
AUTH_TOKEN=
```

| Variable     | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| `VENUE_ID`   | Resy's venue ID.                                                       |
| `DATE`       | The `YYYY-MM-DD` format for the meal you want to stuff your face with. |
| `EARLIEST`   | The earliest time, in 24-hr format, you're willing to eat.             |
| `LATEST`     | Same as above: how late is too late to sit down?                       |
| `PARTY_SIZE` | 🎵 All by myself... 🎵 (it's an `int`)                                 |
| `PAYMENT_ID` | You'll need this from your account. More details below.                |
| `AUTH_TOKEN` | Same as above — just a JWT you can easily find.                        |

These instructions are current as of 12/29/2024.

### Venue ID

This is the ID of the restaurant you want to eat at. You can find this by going to the Network tab in your browser's
inspector and searching for `venue?filter` after navigating to the restaurant's page. In the case of COQODAQ, this was a 5 digit value (76033).

### Payment ID

You'll need to find your payment ID. This is a little tricky, but not too bad. Again, in the Network tab, find the
request that's made after you authenticate. You can search for `payment_method` in the requests and find the object called `user` from api.resy.com/2/user.
A `payment_method_id` field is in the object and the value is what you want.

### Auth Token

This is easier to find. To view cookies in Chrome, open Chrome Settings (right-click on your browser window) > Inspect > Applications > Storage > Cookies, and select the https://resy.com cookie.
Find the `authToken` name, you want the value. This does expire after a while, so you'll need to update it every so often.

## Usage

After adding your .env configuration file, you have two options.

If you are on a **Windows OS**, you can run the .bat file in the utils directory, and you can use Windows Task Scheduler to import the .xml file so that the script is run on a frequent basis.
Check the logs directory for output from the job. If it is successful, you'll likely get an email from Resy with your reservation details as well.

On other OSes, you can run a script with:

```bash
npm run start
```

This will trigger a bash file called `env_manager.sh` that will set the date in the `.env` to two weeks from now (when
most restaurants start opening up reservations) and then run the script. However, you can modify it to run on the `.env`
as is by setting the date manually and then running:

```bash
npm run start:today
```
