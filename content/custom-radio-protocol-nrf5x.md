---
author: "Jason Wright"
title: "Writing a custom radio protocol for nRF5x"
date: "2021-09-20T10:52:59+08:00"
toc: true
categories: ["nrf5x", "firmware", "wireless"]
---

The nRF5x family of wireless MCUs from Nordic Semiconductor supports a wide array of commonly used protocols - Bluetooth LE, ANT, ESB, for example - but for certain applications a custom protocol is desirable to further optimize power consumption, throughput, or latency beyond existing offerings.

In this post I will walk through implementing a simple custom radio protocol for nRF5x chips. Called CHIRP (custom highly-interesting radio protocol), the idea is for a bare-bones lowest-power protocol: a short (8-byte) packet will be transmitted once every 5 seconds, after which there will be a short window (500 us) to receive a response.

## Getting started

This example uses the nRF5 SDK for register definitions and the RTC library. However, it should be easily portable to nRF Connect SDK projects. Because we are implementing a custom protocol, the SoftDevice should not be used.

As for the nRF5 SDK examples, you need the following tools installed:

* GNU Make
* nRF Command Line Tools

I will test using the nRF52 DK (PCA10056), but any development board should do.

> If you are using a board with no external RTC crystal, be sure to edit

> To fully test the protocol you'll need two boards - one transmitter and one receiver.

The full code for this example is available as a Git repo here.

## Initializing the RADIO peripheral

## Putting SHORTS on

## Setting up the RTC

## Comparing CHIRP to other protocols

CHIRP is intended to be a "lowest-power" protocol, sending a miniscule amount of data (only 1.6 Bps) in order to keep radio usage and power consumption at a minimum. Let's see how the power consumption compares to a BLE peripheral that updates a characteristic once every 5 seconds. We will ignore the power consumption during broadcasting & connecting to the BLE peripheral, because CHIRP is always transmitting.

The nRF Power Profiler is used for measuring the current draw from a nRF52 DK board.

### Measuring power consumption

## Drawbacks of using a custom protocol