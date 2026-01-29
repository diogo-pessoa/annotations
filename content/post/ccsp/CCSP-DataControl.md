---
title: "CCSP DataControl - Notes"
date: 2024-09-07T14:15:21+01:00
draft: false
toc: true
tags:
  - ccsp
  - Data-management

---

## Data Ownership

### Data Owner responsibilties

`Data owners`, the individuals responsible for classifying, protecting, and overseeing the use of
data
in organizations, should also be careful to restrict permissions for modifying and processing their
data; users should be limited to those functions that they absolutely require in order to perform
their assigned tasks. And, as in many circumstances in both the cloud and legacy environments,
logging and audit trails are important when data is being manipulated in any fashion. (
p.67) [1](#references)

### Cloud Providers responsibilties

`Cloud providers` need to ensure that they provide secure environments for data use as well. That
means strong protections in the implementation of virtualization or shared services. Providers must
ensure that data on a virtualized host can't be read or detected by other virtual hosts on that same
device. They also have to implement personnel and administrative controls so data center personnel
can't access any raw customer data and so their actions are monitored in case of malicious activity.
These controls are most often ensured via contractual language and third-party audits when working
with cloud providers.

## Data anonymization

Data anonymization is the process of protecting private or sensitive information by erasing or
encrypting identifiers that connect an individual to stored data.

## Data Destruction/Disposal

### Crypto-Shredding (Cryptographic Erasure)

This involves encrypting the data with a strong encryption engine and then taking the keys generated
in that process, encrypting them with a different encryption engine, and destroying the resulting
keys of the second round of encryption. Crypto-shredding is considered a better solution than
overwriting because data that is encrypted from the beginning of its lifecycle and then shredded
cannot be recovered even if remnant data remains. The primary downfall of crypto-shredding is due to
the CPU and performance overhead of encrypting and decrypting files. _(p. 56) CCSP Exam
Guide_[1](#references)

### Degaussing

This involves applying strong magnetic fields to the hardware and media where the data resides,
effectively making them blank. It does not work with solid-state drives like SSDs, flash media, and
USB thumb drives.

## References

1. Mike Chapple. Mike Chapple, David Seidl - ISC2 CCSP Certified Cloud Security Professional
   Official Study Guide-Sybex (2022)
