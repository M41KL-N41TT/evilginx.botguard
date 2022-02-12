<div align="center" style="padding: 100px;">
  <img alt="Gopher" src="https://www.upload.ee/image/13836698/XlhdIMk_sml_sml.png" height="100" />
    <img alt="Evilginx" src="https://raw.githubusercontent.com/kgretzky/evilginx2/master/media/img/evilginx2-title-black-512.png" height="60" />
  <img alt="Dot" src="https://fontmeme.com/permalink/220128/1d7d530a9125676cd8dd5f505cc69831.png" height="10" />
    <img alt="Botguard" src="https://fontmeme.com/permalink/220128/bb533f894a48dd9253154f24a45f00d6.png" height="60" />
    <img alt="Yao Ming Face" src="https://www.pngall.com/wp-content/uploads/2016/05/Yao-Ming-Face-PNG.png" height="80" s />
</div>


This repo will describe a method which allows you to successfully use MITM phishing tools on Google's login page without JS raising a fuss and [blocking login](https://i.stack.imgur.com/MnjWd.png). 

## Method

To succesfully log in via phishing page, you'll need to submit a **valid** botguard token in the /accountLookup request.

You can see `bgRequest=` in request params after submitting email, this is the botguard token.
Retrieve a valid token and replace the non-working token with the new, valid token.
Tokens are tied to specific accounts. A way to get a valid token, is to simply generate one on the real google.com page by trying to log in.

I used a headless browser, [go-rod](https://github.com/go-rod/rod) to retrieve the token. It will visit accounts.google.com, enter the victim's email and retrieve the token from request params.

## Notes

The previously described method takes about 4-8 sec. I've sped it up by making a REST API which retrieves the token. It initially takes 2-4 sec to start the browser and further requests take 0.2 sec on average. If you can work out a way to use the same page for each request, the time may possibly drop down to 0.08 sec. 

It's also worth noting that you do not have to send out the headless browser's request, since the botguard token is generated clientside via JS. Hijacking and blocking the request is a good approach to avoid being throttled and/or captcha'd.

As previously described, the botguard token is generated clientside via JS. To make it you require some form of binary which get executed at custom javascript VM. The resulting token *might* be an encrypted string containing personally identifiable information, fingerprints, JS variables declared outside the VM, mouse data etc.

The issue at hand with why it doesn't work by default with evilginx (and other tools) is because the domain differs from google.com. If you use google.com as your phishing domain and add "127.0.0.1 google.com" to your /etc/hosts file, the login process on the phishing page works flawlessly. You can get the credentials and auth tokens. 

Google's botguard detects headless browser sessions. You have to use puppeteer's stealth-evasions via [go-rod/stealth](https://github.com/go-rod/stealth). It's possible that more ways to detect headless browsers will be added by Google in the future, ergo more evasion methods must be made.

## Up to now

![current](./current.png)

## Working exploit

![botguard](./botguard.png)


## Contact

Feel free to reach out to my email address listed on my GitHub profile if you want to hire me, say thanks, or point out inaccuracies/mistakes I made.
