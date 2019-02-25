---
layout: post
permalink: /simple-guide-for-sebak-migration/
---
---
# Simple guide to Exchanges for BOScoin migration

Dear exchange developers.
Hello.
Now we are happy to announce BOScoin's main net ( A.K.A SEBAK) will launch in near future. 
For the migration in a smooth, Please read below introduction carefully.

For Help to understanding, BOScoin team write down simple guide. You can find specific guide in below link. 

https://github.com/bosnet/sebak/wiki/For-exchanges

Here is the BOScoin SEBAK's testnet URL address.
http://testnet-sebak.blockchainos.org/

Of course you can access when you opend above link aned to send transaction. However before send transaction, you must check SEBAK api at first. 

Here is the [SEBAK API document](https://bosnet.github.io/sebak/api/).

So We,BOScoin dev team, **stronly recommend** to test in your local enviroment. You can see the contents [How to test in your local enviroment](https://github.com/bosnet/sebak/wiki/Running-Standalone-Mode).

At the moment, BOScoin dev team provide SDKs in Python and Javascript. Please check below repositories. 

[sebakjs-util](https://github.com/bosnet/sebakjs-util): Sebak SDK in Javascript

[sebakpy-util](https://github.com/spikeekips/sebakpy-util): Sebak SDK in Python

Before send transaction, you are necessary to make account. For that you have to use tool,called 'sebak-angelbot'. 
Here is the [sebak-angelbot repository](https://github.com/spikeekips/sebak-angelbot)

And here is the BOScoin team's angelbot url address to provide.
https://testnet-angelbot.blockchainos.org/

NOTE: when you send transaction, Please take care of **BOScoin's measure of amount**. 
 BOScoin unit should be **GON**. It is decimal unit in BOScoin like as Satoshi in the Bitcoin and Gwei in the Ethereum. 
**So 1 BOS equals to 10,000,000 GON.**

# Q&A inquiries

When you met issues during test on the [Public test net](http://testnet-sebak.blockchainos.org/),Please send email to dev-support@blockchainos.org.

Email must include below informations.
- Your error results or questions
- Where did you find errors, in Testnet or your own environment.
- SEBAK version and commit id.
- If you request something to SEBAK, the payload you sent.

Thank you.

BOScoin dev team





