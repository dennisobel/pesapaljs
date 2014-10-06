##PesapalJS

[![NPM](https://nodei.co/npm/pesapaljs.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.org/package/pesapaljs)

#####Important: This code is not ready for production use!!

######Goal

Make it easy to integrate [PesaPal](https://www.pesapal.com) into a website or mobile app AND most importantly allow one 
to customize the payment user interface.

######Features

- Prepare signed URL for payment page.
- Custom payment page

######Usage summary
    var PesaPal = require('pesapaljs');
    PesaPal.initialize({key: CONSUMER_KEY, secret: CONSUMER_SECRET});
    
    // Listen for payment notifications (With an express app)
    app.get('/ipn', PesaPal.listen, function(req, res){ 
        var error = req.pesapal.error;
        var payment = req.pesapal.payment; // {transaction, method, status, reference}
        // do shit 
    });
    
    // ...
    
    // Payment Info
    PesaPal.paymentStatus({reference:"REF78D"}, callback);
    PesaPal.paymentDetails({reference:"REF78D"}, callback);
    
    // ...
    
    // Direct order
    var customer = new PesaPal.Customer("kariuki@pesapal.com");
    var order = new PesaPal.Order("REF78D", customer, "Ma ndazi", 1679.50, "KES", "MERCHANT");
    
    // Redirect user to PesaPal
    var url = PesaPal.getPaymentURL(order, "http://mysite.co.ke/callback");
    
    // or place order directly with your own UI
    PesaPal.makeOrder(order, callback, PesaPal.PaymentMethod.MPesa);
    
        //... in your callback render a UI to collect payment details (web or mobile)
    
    //... then somewhere in your callback after you've collected card details 
    //      (or mobile money transaction code)
    var mobileMoney = new PesaPal.MobileMoney("254718988983", "FRTSFTTY56");
    PesaPal.payOrder(order, anotherCallback,  mobileMoney);
