---
title: "Take a first step of Azure Quantum"
categories:
  - Azure
tags:
  - Azure
  - Q Sharp
  - Cloud
last_modified_at: 2022-04-23
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

Let's submit your first Q# program through Azure Quantum.

## Before your first Q# program

To make a Q# program, you may need these tools:

- [.NET Core 3.1 version (both SDK and Core)](https://dotnet.microsoft.com/en-us/download/dotnet/3.1)
- [Visual Studio Code](https://code.visualstudio.com/download)
- [Microsoft Quantum Development Kit for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=quantum.quantum-devkit-vscode)

These things will help you execute your Q# programs locally in Visual Studio Code. Please install to follow steps below.

## Create a QRNG project

First of all, open Visual Studio Code and click `Command Palette...` in the `View` tap.

![command_palette](https://user-images.githubusercontent.com/62553200/164894678-73142f01-760b-4c83-a562-1d5dff0ea1c1.png)

In the search box, enter `Q#: Create new project...` and click it.
I can see it before I typed it because I've tested it before to write this post.

![new](https://user-images.githubusercontent.com/62553200/164894677-9da8f88e-4c01-4161-8f65-c792b616d474.png)

Then, select `Standalone console application` and enter your own project name. If you've done to write it, select `Create Project`.

![created](https://user-images.githubusercontent.com/62553200/164894675-c15d7ab4-6919-49ff-abf8-da53fe825e36.png)

After a while, you can see a pop-up window at the right below.
Please Click `Open new project...` in that window.
It will let you open the project folder you've just maken.

If you successfully open this project, you can see two files: `Program.qs` and `HelloWorld.csproj`.
The `.qs` file is the main program and the other is for project settings.
Double click this Q# file and open it to execute.

![program.qs](https://user-images.githubusercontent.com/62553200/164894674-e27c70b8-79be-4cc5-bd23-71e35c984c91.png)

1. Click `New Terminal` in the `Terminal` tap
2. Run `dotnet run`

	```
	dotnet run
	```

If the program executes well, it may print out a string: `Hello quantum world!`.
If there is an error, please make sure that you are in the project folder that you make from jobs above.
However, this program is boring. Let's make it more interesting!

Using one of quantum gates, you can generate a real random number.
To get this number, you need a qubit and a hardmard gate.
If you measure a qubit with the hardmard gate, the result is 0 or 1 with a 50/50 chance.
This is called superposition which is a characteristic of qubits.
This program is often called as QRNG which means Quantum Random Number Generator.

Now change codes in `Program.qs`.

```qsharp
namespace QRNG {

    open Microsoft.Quantum.Canon;
    open Microsoft.Quantum.Intrinsic;
    // import measurement
    open Microsoft.Quantum.Measurement;
    
    @EntryPoint()
    operation GenerateRandomBit() : Result {
        // Make a qubit
        use q = Qubit();
        // Put the qubit to superposition
        H(q);
        // It now has a 50% chance of being measured 0 or 1
        // Measure the qubit value
        return M(q);
    }
}
```

Execute it the same way before, then you can see it prints out `Zero` or `One` randomly like the image below.

![dotnet_run](https://user-images.githubusercontent.com/62553200/164894681-de35531c-2f4d-4f83-a595-0a8e0e412e33.png)

What did you get from your first QRNG program? Zero or One?

## Use real devices in Azure Quantum

Now you may have a QRNG program and execute it serveral times in your local.
However, every result that you got are not from actual quantum computers because you executed the QRNG program with a simulator of QDK in your local.
The simulator imitates real devices so it enables you to run quantum programs locally.
This is definitely useful to learn quantum computing and test your program, but some of you may be curious the result from actual one.
I will share how to send a job to real quantum computers below.

> To use real quantum devices in Azure, you need an Azure account. Please prepare one.

1. Install [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) (.NET Core SDK 3.1 not required)
2. Install the Azure CLI quantum extension executing `az extension add -n quantum` in the terminal
  ![extension](https://user-images.githubusercontent.com/62553200/164894671-88870777-c57b-449b-9808-6807ba3a7a7f.png)
4. Open `.csproj` file and add `<ExecutionTarget>ionq.qpu</ExecutionTarget>` between two `<PropertyGroup>`
5. Enter `az login` in the terminal and login your Azure account through a web browser
6. Enter `az account set -s YourSubscriptionID` in the terminal
7. (It takes time) Make a Azure Quantum workspace through [the Azure Portal](https://azure.microsoft.com/en-us/features/azure-portal/)
8. Enter `az quantum workspace set -g YourResourceGroup -w YourWorkspace -l YourLocation -o table` and wait for the table output *IMPORTANT: write `YourLocation` in **mixed case surrounded by quotes** or in **lower case with no spaces or quotes**
10. Enter `az quantum target list -o table` to check available providers
  ![table](https://user-images.githubusercontent.com/62553200/164894680-a6d3e1a0-37bd-4420-b688-99fe6577bd82.png)
11. Enter `az quantum job submit --target-id ionq.qpu -o table` to submit a job *You can replace `ionq.qpu` with **any of the ones in Target-id**

Enter `az quantum job show -o table --job-id YourJobID` to check the status of submitted job.
If the status is changed to `Succeeded`, you can see your output to enter the command below.

```
az quantum job output -o table --job-id YourJobID
```

I get this result from Azure.

![final_result](https://user-images.githubusercontent.com/62553200/164894679-b35d4ffb-959a-4537-804e-8b9b68c11a99.png)

The frequency is not accurately 50/50 because there is quantum noise for the actual devices.

## References

- [Exercise - Install the QDK for Visual Studio Code](https://docs.microsoft.com/en-us/learn/modules/qsharp-create-first-quantum-development-kit/2-install-quantum-development-kit-code)
- [Quickstart: Create a quantum-based random number generator in Azure Quantum](https://docs.microsoft.com/en-us/azure/quantum/quickstart-microsoft-qc?pivots=platform-ionq)
- [Exercise - Create a quantum random bit generator](https://docs.microsoft.com/en-us/learn/modules/qsharp-create-first-quantum-development-kit/3-random-bit-generator)

---

💬 _Any comments and suggestions will be appreciated._
