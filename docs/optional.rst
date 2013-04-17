Authorize Transaction Details
=============================

With each transaction, authorize.net provides the ability to specify different information about the operation.

This information can show up on a customer's bill; more and more customers are expecting to see detailed information on their statements.

Additionally it can be helpful to maintain accounting, resolve discrepencies or customer inquiries.

Transaction Details
-----------------------

.. _subscription_details:

Subscription Details
+++++++++++++++++++++++++++

The default interface exposes the minimum necessary to create a recurring transaction: :ref:`recuring_charge`

The full data structure provides a lot of additional information and may be necessary so that Authorize doesn't prevent you from creating multiple recurring charges for an individual.

Uniqueness is defined by the fields listed :ref:`subscription_uniqueness`

Example
~~~~~~~~~~~~~~~~~~~~~~~~~

Setting an order description is fairly common as it shows up on a customer's credit card statement::

    >>> from datetime import date
    >>> card = client.card(cc)
    >>> order_type = client.factory.create('OrderType') 
    >>> order_type.invoiceNumber = '1234'
    >>> order_type.description = 'Monthly recurring charge from ACME Corporation'
    >>> details = { 'order': order_type }
    >>> card.recurring(20, date(2012, 12, 1), months=1, extra=details)
    <AuthorizeRecurring 1396734>

Data Structure
~~~~~~~~~~~~~~~~~~~~~~~~~

      (ARBSubscriptionType){
         name = None
         paymentSchedule = 
            (PaymentScheduleType){
               interval = 
                  (PaymentScheduleTypeInterval){
                     length = "<< Populated by AuthorizeSauce >>"
                     unit = 
                        (ARBSubscriptionUnitEnum){
                           value = "<< Populated by AuthorizeSauce >>"
                        }
                  }
               startDate = "<< Populated by AuthorizeSauce >>"
               totalOccurrences = "<< Populated by AuthorizeSauce >>"
               trialOccurrences = "<< Populated by AuthorizeSauce >>"
            }
         amount = "<< Populated by AuthorizeSauce >>"
         trialAmount = "<< Populated by AuthorizeSauce >>"
         payment = 
            (PaymentType){
               creditCard = 
                  (CreditCardType){
                     cardNumber = "<< Populated by AuthorizeSauce >>"
                     expirationDate = "<< Populated by AuthorizeSauce >>"
                     cardCode = "<< Populated by AuthorizeSauce >>"
                  }
            }
         order = 
            (OrderType){
               invoiceNumber = None
               description = None
            }
         customer = 
            (CustomerType){
               type = 
                  (CustomerTypeEnum){
                     value = None
                  }
               id = None
               email = None
               phoneNumber = None
               faxNumber = None
               driversLicense = 
                  (DriversLicenseType){
                     state = None
                     number = None
                     dateOfBirth = None
                  }
               taxId = None
            }
         billTo = 
            (NameAndAddressType){
               firstName = "<< Populated by AuthorizeSauce >>"
               lastName = "<< Populated by AuthorizeSauce >>"
               company = None
               address = None
               city = None
               state = None
               zip = None
               country = None
            }
         shipTo = 
            (NameAndAddressType){
               firstName = None
               lastName = None
               company = None
               address = None
               city = None
               state = None
               zip = None
               country = None
            }
       }







Uniqueness
---------------

.. _subscription_uniqueness:


Subscription
####################

These are the fields that Authorize uses to check uniqueness of a subscription. 

   subscription.Article.MerchantID
   subscription.Article.CustomerInfo.Payment.CreditCard.CardNumber
   subscription.Article.CustomerInfo.Payment.eCheck.RoutingNumber
   subscription.Article.CustomerInfo.Payment.eCheck.AccountNumber
   subscription.Article.CustomerInfo.CustomerID
   subscription.Article.CustomerInfo.BillingInfo.BillToAddress.FirstName
   subscription.Article.CustomerInfo.BillingInfo.BillToAddress.LastName
   subscription.Article.CustomerInfo.BillingInfo.BillToAddress.Company
   subscription.Article.CustomerInfo.BillingInfo.BillToAddress.StreetAddress
   subscription.Article.CustomerInfo.BillingInfo.BillToAddress.City
   subscription.Article.CustomerInfo.BillingInfo.BillToAddress.StateProv
   subscription.Article.CustomerInfo.BillingInfo.BillToAddress.Zip
   subscription.OrderInfo.Amount
   subscription.OrderInfo.Invoice
   subscription.Recurrence.StartDate
   subscription.Recurrence.Interval
   subscription.Recurrence.Unit 