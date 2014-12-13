# BitPay Library for Ruby [![](https://secure.travis-ci.org/bitpay/ruby-client.png)](http://travis-ci.org/bitpay/ruby-client)
Powerful, flexible, lightweight interface to the BitPay Bitcoin Payment Gateway API.

## Installation

    gem install bitpay
    
In your Gemfile:

    gem 'bitpay', :require => 'bitpay'

Or directly:

    require 'bitpay'

## Configuration

The bitpay client creates a cryptographically secure connection to your server by pairing an API code with keys generated by the library. The client can be initialized with pre-existing keys passed in as a pem file, or paired if initialized with a pem file and a tokens hash. Examples can be found in the cucumber step helpers.

## Basic Usage

### Pairing with Bitpay.com

To pair with bitpay.com you need to have an approved merchant account.  
1. Login to your account  
2. Navigate to bitpay.com/api-tokens (Dashboard > My Account > API Tokens)  
3. Create a new token and copy the pairing code.  

### To create an invoice:

    client = BitPay::Client.new
    client.pair_pos_client(<pairing_code>)
    invoice = client.create_invoice (price: <price>, currency: <currency>)

With invoice creation, `price` and `currency` are the only required fields. If you are sending a customer from your website to make a purchase, setting `redirectURL` will redirect the customer to your website when the invoice is paid.

Response will be a hash with information on your newly created invoice. Send your customer to the `url` to complete payment:

    {
      "id"             => "DGrAEmbsXe9bavBPMJ8kuk", 
      "url"            => "https://bitpay.com/invoice?id=DGrAEmbsXe9bavBPMJ8kuk",
      "status"         => "new",
      "btcPrice"       => "0.0495",
      "price"          => 10,
      "currency"       => "USD",
      "invoiceTime"    => 1383265343674,
      "expirationTime" => 1383266243674,
      "currentTime"    => 1383265957613
    }

There are many options available when creating invoices, which are listed in the [BitPay API documentation](https://bitpay.com/bitcoin-payment-gateway-api).

To get updated information on this invoice, make a get call with the id returned:

    invoice = client.get_public_invoice(id: 'DGrAEmbsXe9bavBPMJ8kuk')

## Testnet Usage

During development and testing, take advantage of the [Bitcoin TestNet](https://en.bitcoin.it/wiki/Testnet) by passing a custom `api_uri` option on initialization:

    BitPay::Client.new(api_uri: "https://test.bitpay.com/api")

## API Documentation

API Documentation is available on the [BitPay site](https://bitpay.com/api).

## Running the Tests
The tests require that environment variables be set for the bitpay server, user name, and password. First run:
    
    $ source ./spec/set_constants.sh https://test.bitpay.com <yourusername> <yourpassword>
    $ bundle install
    $ bundle exec rake

Tests are likely to run up against rate limiters on test.bitpay.com if used too frequently. Rake tasks which interact directly with BitPay will not run for the general public.

## Found a bug?
Let us know! Send a pull request or a patch. Questions? Ask! We're here to help. We will respond to all filed issues.

## Contributors
[Click here](https://github.com/bitpay/ruby-client/graphs/contributors) to see a list of the contributors to this library.
