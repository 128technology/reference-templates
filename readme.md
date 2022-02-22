## Intro to templating in 128T

### Description

The goal of this repo is to give a user an understanding of how to leverage advanced templating as applied to 128T config.\
If you're a beginner to templating, click on the link to learn more on the open-source [Liquid templating language](https://shopify.github.io/liquid/basics/introduction/).

Prerequisites: Understand concepts such as conditional statements and loops

This guide has 5 parts, it starts with a sample config as shown in [base_template](base_template) and incrementally adds more logic to the template to make it more robust, ease of user management, etc.

[Part 1](Part_1/readme.md)

Goal: Identify / Mark variables

In here, we take the `base_template` and identify site-to-site variables and mark those fields to be sourced from the `variables`.

[Part 2](Part_2/readme.md)

Goal: Use `if-else` conditions and `loops` within the template.

Here, we take the add-on to the template from Part 1 and add `if else` logic. In this example, we automatically configure the right pci address in the configuration template given a Hardware identifier in the variables tab.

[Part 3](Part_3/readme.md)

Goal: Deleting config elements.

Here, we see two different types of delete syntax needed depending on the config element.

[Part 4](Part_4/readme.md)

Goal: Emulate a dictionary within the template

[Part 5](Part_5/readme.md)

Goal: Use network filters to determine IPs in a network
