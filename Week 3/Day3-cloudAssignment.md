## Assignment 1
- Install AWS CLI
- Connect , Authorize to your AWS Account through it( Mobaxterm) .
- List or create s3 bucket or any ec2 instance using AWS Code Documentation.

## Mobaxterm Code I used to complete it.
---
After 1 hour of trial and error and reading documentation, I got this  , will work on it tommorow again...


  2026-03-28   03:14.09   /home/mobaxterm  aws configure
AWS Access Key ID [****************UECJ]:
AWS Secret Access Key [****************tE1Y]:
Default region name [us-east-1]:
Default output format [JSON]:
                                                                                                         ✓

  2026-03-28   03:14.54   /home/mobaxterm  aws s3 ls

aws: [ERROR]: An error occurred (AccessDenied) when calling the ListBuckets operation: User: arn:aws:iam::978820702462:user/ashu66-tem is not authorized to perform: s3:ListAllMyBuckets with an explicit deny in an identity-based policy
                                                                                                         ✗

  2026-03-28   03:15.07   /home/mobaxterm 


---


# `Solved it the next day` 

---

## What Was Happening & How You Fixed It

### The Two Separate Problems

You actually hit **two completely different errors** back-to-back, which is why it was so confusing and time-consuming.

---

### Problem 1: `SignatureDoesNotMatch` (first ~2 hours)

This is the error you kept hitting with `aws s3 ls` initially.

**What it means:** AWS received your request but the cryptographic signature it calculated didn't match what you sent. This is almost always a **secret key formatting issue**.

**What was causing it:** Look closely at your `aws configure` attempts:

```
# Attempt 1 — space AFTER the colon (extra space before key)
AWS Secret Access Key [****************tE1Y]: BU
                                            ^
                                         space here

# Attempt 2 — no space (pasted correctly)  
AWS Secret Access Key [****************tE1Y]:BUs
```

When you typed the key with a **leading space**, AWS CLI stored ` BUs112...` (with a space prepended) as your secret key. Since the secret key is used to cryptographically sign every request, even one extra character makes the signature completely wrong. AWS couldn't verify your identity at all.

The third `aws configure` attempt had a space **after** the colon again, but this time it worked — which suggests the CLI might have trimmed it, or you hit Enter in a way that didn't include it.

---

### Problem 2: `AccessDenied` — explicit deny in identity-based policy

Once the signature started working, you hit a **completely different wall**:

```
User: arn:aws:iam::97:user/asu66-tem is not authorized 
to perform: s3:ListAllMyBuckets with an explicit deny in an identity-based policy
```

**What it means:** Your credentials were now valid and AWS recognized you — but an IAM policy attached to your user (`ahu66-tem`) explicitly **denied** `s3:ListAllMyBuckets`. 

The key word here is **explicit deny**. In AWS IAM:

- A missing Allow = access denied (default)
- An **explicit Deny** = access denied and **cannot be overridden** by any Allow, even from another policy

**How you fixed it:** You went into the AWS Console → IAM → found the identity-based policy attached to `ashu66-tem` → removed it. Once that explicit Deny was gone, your user's underlying permissions (or a broader Allow) kicked in, and `aws s3 ls` returned your bucket `clitest-s3`.

---

### The Full Timeline

| Phase | Error | Root Cause | Fix |
|---|---|---|---|
| Hour 1-2 | `SignatureDoesNotMatch` | Leading/trailing space in secret key during `aws configure` | Re-ran `aws configure` carefully without extra spaces |
| Hour 2-3 | `AccessDenied` (explicit deny) | IAM identity policy explicitly denying `s3:ListAllMyBuckets` | Removed the policy from AWS Console |
| End | ✅ `2026-03-26 clitest-s3` | — | — |

---

### Key Lessons

**On the signature error:** Always paste secret keys carefully. If you suspect a bad key is stored, you can verify what's saved by running `aws configure list` — it shows your config without exposing the full secret.

**On the explicit deny:** In AWS IAM, explicit denies are the highest priority rule. If you ever see "explicit deny in an identity-based policy," no amount of adding Allow rules will fix it — you *must* find and remove or modify that Deny statement. The error message even tells you it's identity-based (vs. resource-based), which narrows where to look.

---
