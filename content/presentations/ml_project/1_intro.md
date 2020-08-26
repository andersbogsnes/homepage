---
weight: 10
outputs: ["Reveal"]
---

{{% section %}}

{{< slide background-image=https://www.themagichelpers.com/wp-content/uploads/2020/07/banner-new-scaled.jpg >}}

<div style="background-color: #f2f2f2cf">
    <h1 style="color: black">The Project</h1>
</div>

---

{{< slide background-image=https://www.themagichelpers.com/wp-content/uploads/2020/07/banner-new-scaled.jpg >}}

<div style="background-color: #f2f2f2cf; color: black">
    <h2> The Scenario</h2>
    <ul>
      <li>We are a new Copenhagen-based startup, homehelper.dk</li>
      <li>We are a fully managed AirBnB service for people who want to rent their apartment on AirBnB</li>
      <li>We offer cleaning services, pictures, key exchange and everything else a potential renter might need</li>
    </ul>
</div>

---

{{< slide background-image=https://www.themagichelpers.com/wp-content/uploads/2020/07/banner-new-scaled.jpg >}}

<div style="background-color: #f2f2f2cf; color: black">
    <h2>The Product</h2>
    <p>We want to build a feature where a potential renter can answer a few questions about their apartment and get an indication of how much money they can expect to make (minus our fee, of course!)</p>
</div>

---

## The data

We are a startup, so we don't have much data. Luckily, we found a website that scrapes AirBnB data!

http://insideairbnb.com/

---


building dataset
    - Include data download

preprocessing
    - Setting up pipelines
    - Custom transformers

training
    - Hyperparam search
    - model selection
    - feature selection

evaluating
    - Plotting

CLI
    - argparse
    - click

tox

{{% /section %}}