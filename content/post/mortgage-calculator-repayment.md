---
title: "Mortgage Calculator Repayment"
date: 2024-09-12T10:07:58+01:00
description: "Calculate the best period for a mortgage repayment, to optmize interest rate"
featured: true
draft: false
toc: true

math: true
categories:
  - finance
tags:
  - mortgage
---

## Summary

Calculate the best period for a mortgage repayment, to optmize interest rate... `<WIP>`

## Calculator

{{ partial "mortgage-calculator.html" . }}

{{ if or .Params.math .Site.Params.math }}
{{ partial "math.html" . }}
{{ end }}