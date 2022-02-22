## Part 1



In this first segment, we take the [base_template](../base_template) and identify the **variables**.\
Here, **variables** refer to the site-to-site variable fields, like the **router / branch name, node name, branch ip, etc**.\
The value of the variables are sourced from the variable file / tab and populated into the template configuration during the render/generate phase.

In the sample [template](template) here, we identify the fields that are variables and tag them as shown below:

| Static Values          | Variables                     |
| ---------------------- | ----------------------------- |
| **bostonsite1**        | **{{ instance.routerName }}** |
| **+47.6062-122.3321/** | **{{ instance.location }}**   |
| **branchoffice1**      | **{{ instance.nodeName }}**   |


The **instance.routerName**, **instance.location** and **instance.nodeName** are sourced from the [variables](variables.json) file / tab and populated within the template.

> NOTE: We remove auto-generated config elements such as **adjacency** etc from the template.
