# TryHack3M: Subscribe Writeup
### Lab Link [TryHack3M: Subscribe](https://tryhackme.com/room/subscribe)
<img width="1200" height="1200" alt="image" src="https://github.com/user-attachments/assets/9b466c2d-043a-4e54-8764-ab0800bd85e7" />

## Incident Storyline
```
We have good news and bad news! The good news is that we are about to hit 3 million users on our platform, and the bad news is;

Well, last night, the UnderGround (UG) Hackers attacked our website, hackme.thm, and took complete control. They were able to turn off the signup page, so there won't be any new registrations. Given this, our user count is stuck at 2.99 Million.
Can you help us restore the registration panel on our site to reach our 3 million user milestone?
```
<img width="1125" height="562" alt="image" src="https://github.com/user-attachments/assets/bdf6e6fa-07da-4296-b6e4-58e99e470381" />

## Task 2 : Exploitation
`Sometimes, the attacker leaves footprints that allow you to regain access to the server.  Can you help HackM3 restore server access and get 3M subscribers?`

### Q1: What is the invite code for the hackme.thm website?

Once you access the webpage, the following page will appear.

![ah](Images/1.png)

To examine the website details, right-click on the page and choose Inspect from the browser menu.

![ah](Images/2.png)

When attempting to sign up, the following message appears: `Registration is currently disabled! Invite-only access`.

![ah](Images/3.png)

Further analysis within the browser’s Inspect panel revealed an invite.js file located in the Sources tab.

![ah](Images/4.png)































































































































