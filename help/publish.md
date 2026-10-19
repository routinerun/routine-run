# Publish for a code: a routine on a sign

Publish puts one routine under a short code, for a sign, a sticker or a QR at a door. Anyone with the code opens the steps on an iPhone: in the App Clip, with nothing to install, or in Routine Run if they have it.

## Where to find it

A routine's page, the ellipsis, **Publish for a code**. Nothing is published until you press **Publish**.

## What goes, and what does not

Only the steps, as somebody at a counter would walk them:

- **Goes:** the routine's name and description, and each step's name, description, duration, mark and the time of day it is set to.
- **Stays on your phone:** actions and web addresses, triggers, values, notes, places, tags, any calendar date a step is set to, and every step you turned off.

So publish a routine you would put on a sign.

## Where it is kept

In iCloud's public database for Routine Run, under your iCloud account. Anyone with the code can read it, Apple keeps it, and we can see it in iCloud's developer console, because that is how a public database works. With it, iCloud keeps an identifier for your iCloud account: a random string that lets only you change or take it down. It does not tell us your name or your Apple ID. Anyone who opens one of your codes can see that identifier too, so they could tell that two codes came from the same account; the app uses it only to let them block you on their phone.

You need to be signed in to iCloud on the phone. iCloud sync does not need to be on.

## The code

It looks like `routinerun.com/c/ab23cd45`: eight letters and digits, with no 0, 1, i, l or o so it can be read off a sign. The page shows it as a QR, with **Share the link** and **Copy the link**. The link is short enough for any QR, an NFC tag or an App Clip Code.

- **Print a poster** prints a page with the QR and every step written out, for visitors whose phone the code does not open. Letter in the US and Canada, A4 elsewhere.
- **Write to an NFC tag** puts the link on a sticker. An iPhone held to it opens the same steps, with no app running. NTAG213 stickers hold it many times over.

## On your own website instead

An organisation can keep the routine on its own site rather than in iCloud. On the same page, **Share the two files** gives you two files named after the routine:

- `before-the-counter.json`: the routine, with only the steps in it, the same as publishing.
- `before-the-counter.html`: a page listing the steps, for anybody who opens the address without an iPhone.

Put them on your site so the page opens at an address like `https://your-site/rr/before-the-counter` and the routine sits beside it at the same address with `.json` on the end. Then write to support@routinerun.com with the address. We register it, and the code on your door opens the steps with your domain on them. Nothing goes to iCloud this way.

## Update and Unpublish

- **Update** sends the routine as it is now, under the same code. Printed copies keep working.
- **Unpublish** takes it down for everybody, and asks first, because every printed copy stops working. Publishing again makes a new code.

The code is remembered on the phone you published from. If you delete the app or lose that phone, write to support@routinerun.com with the code and we will take it down.

## What is not published, and reporting one

Before anything is published, the phone checks the routine's words against a short list of slurs and strong profanity, and refuses to publish one that carries any of them. It says which word, so you can change it. A phone that opens a code checks the same way and does not show such a routine.

Opening somebody else's code, you can:

- **Report this routine**, on the import screen in the app or under the routine's name in the App Clip. It writes an email to support@routinerun.com from your own Mail, with the code in it. We take down a routine that breaks the rules.
- **Block whoever published it**, on the import screen in the app. Every code from that publisher stops opening on this phone. Opening one of their codes again offers **Unblock and open**. The list stays on your phone.

## Related

- [Export](export.md): send a routine to somebody as a file or a link
- [iCloud sync](icloud-sync.md): your own private iCloud, which is separate from this
