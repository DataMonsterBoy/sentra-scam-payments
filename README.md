# Sentra Bank Ghana: scam payments project

## The situation

You're an intern at **Sentra Bank Ghana**. Some customers are being tricked into sending money to scammers themselves. This is known as authorised push payment (APP) fraud.

The **Head of Product** and the **Head of Data** are **not happy with how the bank catches scams today**. They think it misses most of them. They want to do better, but they don't yet know exactly what "better" looks like.

They're away at a conference for two weeks. When they get back, they want a first conversation with you about the data and what could be done. **Your job is to prepare for that conversation.**

**The remit is deliberately open.** There's no single right answer, and you aren't expected to do everything. A few things done well, with evidence, beat many things done thinly.

This is synthetic data built for the exercise. Treat it as if it were the bank's real data. If something looks odd, note it as a question for the data team and move on.

## How the bank catches scams today

- **Warning screen:** some payments show the customer a scam warning before they confirm. A fixed rule decides which payments get one. The screen was redesigned in the second half of 2025.
- **Fraud team reviews:** the fraud operations team reviews some payments a few days after they've been made.
- **Customer reports:** otherwise, the bank only finds out when a customer (or their family) reports a scam through the call centre, a branch or the app. By then the money has usually gone.

There's no fraud model or scoring today.

## Directions you could take

Pick one or more, and follow what you find.

1. **Understand the data.** Get a solid grasp of what's there, then dig into the parts you think matter most to the bank.
2. **Build something that catches scams.** This could be a model, or simple rules with no machine learning at all. Either way, show how you know it's better than what the bank does today.
3. **Think about putting it live.** How could your idea work in practice, and what would get in the way?

## What to bring to the conversation

A short group presentation:

- **What you found**, and any proofs of concept, with the evidence behind them.
- **What you'd suggest next.**
- **What we don't know that matters:** *"We don't know X. It matters because Y. To find out we'd need Z."*
- **Both people in the room:**
  - The Head of Product cares about customer friction, losses and complaints.
  - The Head of Data cares about what's feasible, how good the data and labels are, and what's missing.
- **How you used AI:** where it helped, and how you checked what it told you.

## About the bank

- **Size and reach:** a retail bank with about 10,000 customers, mostly in Greater Accra and Ashanti.
- **Products:** current and savings accounts for individuals, and business current accounts for small businesses.
- **How customers pay:** the mobile app, internet banking, USSD (dial codes that work on any phone) and branches.
- **Where money goes:** to bank accounts, at Sentra or other banks, and to mobile money wallets (MTN MoMo, Telecel Cash, AT Money).
- **Name check:** before a transfer is confirmed, the customer sees the name registered on the receiving account.
- **Awareness:** we ran a scam awareness campaign for customers in 2025.

## What you get

| File | What it is |
|---|---|
| `sentra_data.zip` | Seven CSV files: customers, accounts, payees, payments, sessions, scam reports, and the links between reports and payments |
| `data_catalogue.md` | What each file and column means, and how the files join |

Unzip `sentra_data.zip` to get a `data/` folder of CSVs. It's about 40 MB zipped and 190 MB unzipped, and works in pandas on a normal laptop. There's no "is fraud" column: deciding what counts as fraud is part of the job.

## Practicalities

- **Groups:** about 5 people.
- **Time:** two weeks.
- **Questions about the data:** [TBC: who to ask, and how]
