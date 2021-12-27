---
title: "Personal air quality monitor"
date: 2021-07-16T11:24:06+08:00
draft: false
type: about
_build:
  list: never
---

Air pollution is [the world’s largest single environmental health risk](http://www.who.int/mediacentre/news/releases/2014/air-pollution/en/).

I have always had an interest in technology to measure and communicate the impact of air pollution on our daily lives. In 2014 I had the great fortune to work as a hardware engineer for the [ICRI Cities](http://cities.io/) group in London, where I primarily contributed to the [London Living Labs project](https://www.london.gov.uk/what-we-do/business-and-economy/science-and-technology/smart-london/smart-london-case-studies/intel), which deployed several hundred environmental monitoring stations across greater London. During that time I also participated in a hardware hackathon in Dublin, and worked on a team to design [Treo](https://thebedlab.com/2014/11/05/2014/11/05/treo-pure-living/), a concept for a wearable air quality monitor.

Upon returning to Intel in Santa Clara, CA I worked on a team engaged in multiple environmental monitoring projects, including [COEL](https://iq.intel.com/womens-health-wearable-developing-world/), a bangle designed to help women in India and Bangladesh reduce carbon monoxide exposure.

### Design

_pemby_ is a wearable air quality monitor intended for urban environments, optimized for cost and ease of use. It has sensors for oxidizing gases (NO_2, O_3), reducing gases (CO, VOCs), temperature, and relative humidity. A key innovation is the use of [metal oxide sensors from SGX Sensortech](https://www.sgxsensortech.com/products-services/industrial-safety/metal-oxide-sensors/), which enables a compact form factor and simple circuit design. It is the same technology used in [AirBeam](http://www.takingspace.org/aircasting/airbeam/). These sensors are simple, low-power, and low-cost. They can’t provide highly-accurate data, but they are suitable for “event detection” applications (like sensing sudden spikes -- for example, pulling up next to an idling truck) and unlike electrochemical gas sensors, they are usable in low-power wearable applications.

pemby streams real time data over Bluetooth LE to a companion smartphone, which then processes that data and displays alerts related to pollutant exposure. The app is also used for periodic calibration of the device’s sensors. The device also contains an RGB LED which can be used to display an “at-a-glance” indication of recent exposure.

pemby uses a rechargeable lithium-ion polymer battery, and only needs to be charged (via standard USB-C charger) about once a week.

The design was done using [Altium Designer 18](https://www.altium.com/) and manufactured by [MacroFab](https://macrofab.com).

<h4>Key component selection</h4>

<p>
  <table>
    <thead>
      <td>Component</td>
      <td>Function</td>
      <td>Cost @ 10 units</td>
      <td>Rationale</td>
    </thead>
    <tr>
      <td>BL652</td>
      <td>Bluetooth module based on nRF52832 SoC</td>
      <td>$7.45</td>
      <td>
        <ul>
          <li>Integrated microcontroller, RF frontend, antenna in low-cost package</li>
          <li>Prior familiarity with nRF52832 and nRF5 SDK</li>
          <li>FCC pre-certified</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>MICS-4514</td>
      <td>MEMS AQ sensor (red/ox)</td>
      <td>$13.32</td>
      <td>
        <ul>
          <li>Dual sensor in single package</li>
          <li>Very compact footprint</li>
          <li>Single 5V supply</li>
          <li>Minimal BOM requirements</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>BME680</td>
      <td>Environmental sensor (tVOC, RH, temperature, pressure)</td>
      <td>$10.78</td>
      <td>
        <ul>
          <li>Low power consumption (~3.7uA @ 1Hz sampling, 0.15uA sleep mode)</li>
          <li>Multiple sensors in single package</li>
          <li>Very compact footprint</li>
          <li>Well-known, quality manufacturer (Bosch's BMP280 barometric pressure is widely used)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>MCP73812</td>
      <td>Single-cell Li-Ion/Li-Polymer charge controller</td>
      <td>$0.56</td>
      <td>
        <ul>
          <li>500mA meets USB2 standard charging spec</li>
          <li>Inexpensive</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>TPS61240</td>
      <td>Step-up DC-DC converter</td>
      <td>$1.54</td>
      <td>
        <ul>
          <li>Required because AQ sensor needs a 5V supply but LiPo only provides 3.3-4.2V</li>
          <li>Small external inductor</li>
          <li>Optimized for Li-Po applications</li>
          <li>Compact package</li>
          <li>High efficiency and automatic power save mode</li>
        </ul>
      </td>
    </tr>
  </table>
</p>

<h4>Schematic & Layout</h4>

<div id="details"></div>

<script src="/js/pdfobject.min.js"></script>
<script>PDFObject.embed("/documents/pemby.pdf", "#details");</script>