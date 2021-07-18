---
title: Parameterize notebooks in Azure Data Studio with Papermill
description: Learn how to create a parameterized notebook in Azure Data Studio by using Papermill.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: vasubhog
ms.author: vabhog
ms.reviewer: mikeray, alayu, maghan
ms.custom: ""
ms.date: 06/18/2021
---

# Create a parameterized notebook by using Papermill

*Parameterization* is the ability to execute the same notebook by using different parameters.

This article shows you how to create and run a parameterized notebook in Azure Data Studio by using the Python kernel.

> [!NOTE]
> Currently, you can use parameterization with Python, PySpark, PowerShell, and .NET Interactive kernels.

## Prerequisites

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## Install and set up Papermill in Azure Data Studio

The steps in this section all run inside an Azure Data Studio notebook.

1. Create a new notebook. Change **Kernel** to **Python 3**:

    :::image type="content" source="media/notebooks-parameterization/install-new-notebook.png" alt-text="Screenshot that shows the New notebook menu option and setting the Kernel value to Python 3.":::

1. If you're prompted to upgrade your Python packages when your packages need updating, select **Yes**:

     :::image type="content" source="media/notebooks-parameterization/update-python-yes.png" alt-text="Screenshot that shows the dialog prompt to update Python packages.":::

1. Install Papermill:

    ```python
    import sys
    !{sys.executable} -m pip install papermill --no-cache-dir --upgrade
    ```

    Verify that it's installed:

    ```python
    import sys
    !{sys.executable} -m pip list
    ```

    :::image type="content" source="media/notebooks-parameterization/install-list-papermill.png" alt-text="Screenshot that shows selecting Papermill in a list of application names.":::

1. You can test to see whether Papermill installed correctly by checking the version of Papermill:

    ```python
    import papermill
    papermill
    ```

    :::image type="content" source="media/notebooks-parameterization/install-validation-papermill.png" alt-text="Screenshot that shows installation validation for Papermill.":::

## Parameterization example

For an example notebook that you can work with in this article, save an [example notebook file](https://github.com/microsoft/sql-server-samples/blob/master/samples/applications/azure-data-studio/parameterization.ipynb), and then open the file in Azure Data Studio:

1. In [GitHub](https://github.com/microsoft/sql-server-samples/blob/master/samples/applications/azure-data-studio/parameterization.ipynb), select **Raw**.
1. Select Ctrl+S or right-click and save the file with the .ipynb extension.  
1. Open the file in Azure Data Studio.

## Set up a parameterized notebook

With the downloaded example file open in Azure Data Studio, complete the following steps to set up a parameterized notebook and try using different parameters. 

1. Verify that **Kernel** is set to **Python 3**:

    :::image type="content" source="media/notebooks-parameterization/change-kernel.png" alt-text="Screenshot that shows the Kernel value to Python 3.":::

1. Make a new code cell. Select **Parameters** to tag the cell as a parameters cell.

    ```python
    x = 2.0
    y = 5.0
    ```

   :::image type="content" source="media/notebooks-parameterization/make-parameter-cell.png" alt-text="Screenshot that shows creating a new parameters cell with Parameters selected.":::

1. Add other cells to test different parameters:

    ```python
    addition = x + y
    multiply = x * y
    ```

    ```python
    print("Addition: " + str(addition))
    print("Multiplication: " + str(multiply))
    ```

    The output will look similar to this example:

    :::image type="content" source="media/notebooks-parameterization/test-cells.png" alt-text="Screenshot that shows the output of cells added to test new parameters.":::

1. Save the notebook as *Input.ipynb*:

    :::image type="content" source="media/notebooks-parameterization/save-notebook.png" alt-text="Screenshot that shows saving the notebook file.":::

## How to execute a Papermill notebook

You can execute Papermill in two ways:

- Command-line interface (CLI)
- Python API

### Parameterized CLI execution

To execute a notebook by using the CLI, in the terminal, enter the `papermill` command with the input notebook, location for the output notebook, and options.

> [!NOTE]
> To learn more, see the [Papermill CLI documentation](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-cli).

1. Execute the input notebook with new parameters:

    ```shell
    papermill Input.ipynb Output.ipynb -p x 10 -p y 20
    ```

    This command executes the input notebook with new values for parameters *x* and *y*.

1. After you enter the new parameters, view the new parameterized notebook. Run all cells to see the new output. A new cell labeled `# Injected-Parameters` contains the new parameter values that were passed in via the CLI:

    :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Screenshot that shows the output for new parameters.":::

### Parameterized Python API execution

> [!NOTE]
> To learn more, see the [Papermill Python documentation](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-the-python-api).

1. Create a new notebook. Change **Kernel** to **Python 3**:

    :::image type="content" source="media/notebooks-parameterization/install-new-notebook.png" alt-text="Screenshot that shows the New notebook menu option and setting the Kernel value to Python 3.":::

1. Add a new code cell. Use `papermill` to use the execute method:

    ```python
    import papermill as pm

    pm.execute_notebook(
    '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Input.ipynb',
    '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Output.ipynb',
    parameters = dict(x = 10, y = 20)
    )
    ```

   :::image type="content" source="media/notebooks-parameterization/python-api-execute.png" alt-text="Screenshot that shows the Python API execution.":::

1. After you enter the new parameters, view the new parameterized notebook. Run all cells to see the new output. A new cell labeled `# Injected-Parameters` contains the new parameter values that were passed in:

    :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Screenshot that shows the output for new parameters.":::

## Next steps

Learn more about notebooks and Parameterization:

- [How to use notebooks in Azure Data Studio](./notebooks-guidance.md)
- [Papermill parameterization docs](https://papermill.readthedocs.io/en/latest/index.html)
- [URI parameterization](./parameterize-uri.md)
- [Run with Parameters](./run-with-parameters.md)