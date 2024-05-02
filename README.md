# iTech | Khurram PoC

This proof-of-concept project is based on 3 parts:

 1. Front-end Development in a Javascript based language
 2. Back-end Development in Python using Flask API
 3. Seamless experience when combined as a full-stack while keeping the front and back end separated as described

The outline of this PoC is based on an estimated 2 day project. This is the same criteria I can complete with a front-end developer I've used before and myself for back-end in a 12 hour period. 

# Overview

This project is an invoicing system that allows the user to issue invoices and accept payment in Bitcoin. The core functionality of this system is described below, but these are the core features:

 1. User's can input their BTC address - this will be used in QR code generation later
 2. User's can upload their company's logo and information
 3. User's can CRUD line items, pricing, etc for the invoice (all pricing should be in BTC, not any specific fiat currency)
 4. When the user is satisfied with the information on the invoice, they generate a PDF that is downloaded

# Front-End

The front-end language must be a javascript based language. Any language that meets that specification may be chosen. Please see the provided Figma file for a design on how the front-end should look. 

## Limitations

Front-end will have limitations placed on what it can do to ensure integration instructions can be strictly followed as well as back-end capabilities are adequate. Please adhere to the following guidelines:

 1. No component of PDF generation can be done in the browser on the front-end. 
 2. Data validation/cleaning must happen in the front-end. For example, checking if inputs are valid or parsing inputs (such as removing spaces, combining inputs, etc).
 3. Math should be done in the front-end as well (such as adding together all line items to get the total price). 

## Data Storage

Data should be stored client-side in a persistent fashion. Anytime the client re-opens the website it should have saved their company name and logo, however line items and other frequently changed data shouldn't be stored. No database is required for this project.

## Input Fields

Basic data validation/cleaning should occur, i.e. numbers are numbers, company name is capitalized correctly, etc. 

## API Calls

API Calls should use HTTPs. If a self-signed certificate is being used for the back-end then this should be reflected and proper handling should occur for a seamless experience. 

## PDF Reports

When a PDF is generated, all PDF information should be sent to the back-end. No PDF generation, QR code generation, etc. may happen on the front-end. PDFs should be generated when a user presses download, and be downloaded from the download link returned by the backend. 

## Deployment

When deploying the front-end there are multiple options:

- Host on Vercel
- Host on Netlify
- Host on an Ubuntu instance

Any deployment of front-end should be strictly separated from the back-end system (for example, no hosting the front-end and back-end on the same AWS instance).


# Back-End

The back-end should be written using Python and the `Flask` module. Other module requirements are defined below.

## Flask API

Endpoints should reflect the need of front-end. Include an extra endpoint that ping/pongs. The Flask API should have a `run.py` file and an `app/` folder structure. 

## Logging

All API endpoints should have a wrapper function for logs. This logging function should save in a JSON file a log of all endpoints that are hit, the time that they are hit, and which IP address hit them.

## PDF Files

All PDF generation should happen on the backend. There should e an API endpoint that accepts all required information and then generates the PDF and hosts the download link, and returns the download link in the response. 

### PDF Generation

When a PDF is generated (see the available Figma design) it should use `PDFKit` and `Jinja2`. The PDF itself should be designed in HTML/CSS with Jinja2 formatting. 

### QR Code Generation

QR Codes should be generated using only the `qrcode` library. They should be scannable and open the users default wallet with the BTC amount and address preloaded. 

## Deployment

When deploying the back-end I would like you to use an Ubuntu server with the following tech:

 1. NGINX proxy that passes through necessary info (think headers, IP address, etc) to the GUnicorn service
 2. GUnicorn running on an internal port that will not conflict with NGINX on restarts
 3. Run them together and create a systemctl service for GUnicorn so that it is persistent with server restarts. 
