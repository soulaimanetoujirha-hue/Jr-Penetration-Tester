## Today's Lesson

**Offensive security** is about thinking like an attacker - finding weaknesses before real hackers do.

**Defensive security** is about keeping hackers out and reacting when things go wrong.

## Tools

- `dirb`

## Usage

`dirb` is used by providing a command and the URL to test.

Example: `dirb https://unrealwebsite.com`

## Today's Lesson (Detailed)

In the cyber world, there are two sides - offensive and defensive security - working side by side in different ways to protect systems or improve their protection.

Every website has a link (http or https + file name + domain) that is public to people. However, some subfiles are hidden and must be protected by a login request or a firewall...

We can find these hidden pages using a tool called `dirb`. It finds pages by trying common words and phrases, such as "admin" or "login".