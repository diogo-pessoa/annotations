---
title: "CCSP NIST" 
date: 2024-11-29T10:26:45Z 
description: "" 
draft: true 
featured: false
toc: false 

codeMaxLines: 10 
codeLineNumbers: false 
figurePositionShow: true 
categories:
  - CCSP
  - CCSP-DATACONTROL
tags:
  - nist
  - data

---

### Media Sanitization Methods

1. Clearing (moderate level)
- Logical overwriting of data using software/firmware-based procedures
- Applies cryptographic erase by leveraging the encryption of target media
- May not work on damaged media or certain storage device types
- Data may still be recoverable with advanced lab techniques

2. Purging (higher level)
- Uses stronger cleaning methods like degaussing for magnetic media
- Applies firmware-based secure erase commands
- Changes the physical state of the storage media
- Makes data recovery significantly more difficult even with advanced lab techniques
- Required for media storing sensitive information

3. Physical Destruction (highest level)
- Physically destroys the media so it cannot be reused
- Methods include:
    - Shredding
    - Disintegration
    - Pulverizing
    - Melting/incineration
- Provides highest assurance of complete data destruction
- Required for highly sensitive/classified data
