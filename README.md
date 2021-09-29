# evilginx.botguard
This repo will describe an exploit which is able to bypass Google's JavaScript login protection which blocks MITM phishing tools.

# What happened here?
The original repository previously contained an exploit kit but I've decided to only share the exploit write-up.

# Method

You'll need to spoof the botguard token used in the /accountLookup request.
You can see `bgRequest=` in request params after submitting email.
You'll need to retrieve a valid token and place it in evilginx's request params before evilginx sends out the request.

I used a headless browser session, [go-rod](https://github.com/go-rod/rod) to retrieve the token. It is set up to visit accounts.google.com, enter the victims email and retrieve the token from request params.
The browser is set to launch after user submits email on phishing page.

# Notes

The previously described method takes about 4-8 sec. I've sped it up by making a REST API which retrieves the token. I used a global browser session and set it up so the API is opening a new page for each request (vs new browser session).
The REST API initially takes 2-4 sec to retrieve the token, further requests take 0.2 sec on average. If you can work out a way to use the same page for each request, the time may possibly drop down to 0.08 sec. It's also worth noting that you do not have to send out the headless browser's request, since the botguard token is generated clientside via JS. Hijacking and blocking the request is a good approach to avoid being throttled and/or captcha'd.

Botguard also detects headless browser sessions, but you can use puppeteer's stealth-evasions via [go-rod/stealth](https://github.com/go-rod/stealth)
# Up to now

![current](./current.png)

# Working exploit

![botguard](./botguard.png)
