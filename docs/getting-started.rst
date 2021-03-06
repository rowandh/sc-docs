###############################
Getting Started
###############################

.. note::
    This section assumes a Windows development environment. Stratis Smart Contracts can be developed on other platforms - documentation is coming soon.

What You'll Need
----------------

* `Visual Studio <https://www.visualstudio.com/downloads/>`_
    The standard IDE for C# development. The Community Edition is available for free.
* `Stratis Smart Contract Visual Studio Solution Template <https://www.visualstudio.com/downloads/>`_
    A template .NET Core solution with a sample contract project and sample unit testing project.
* `Stratis' Command Line Smart Contract Tool <https://www.visualstudio.com/downloads/>`_
    To validate your contracts, and more easily compile and deploy them.

Your First Contract
-------------------

Once the template has been installed, in Visual Studio you can navigate to File > New > Project… and create a new ‘Stratis SmartContract Project’ under ‘Visual C#’.

This will generate a completely new solution with a sample contract - an auction, and a project with some sample unit tests.

You'll notice the class Auction in Auction.cs is just a C# class. If you're not a C# developer you may want to firstly delve into the `basics of C# development <https://docs.microsoft.com/en-us/dotnet/csharp/>`_. This tutorial will focus on the smart contract-specific parts of the code.

::

  using Stratis.SmartContracts;

  public class Auction : SmartContract

The first line in the contract is a reference to the ``Stratis.SmartContracts`` NuGet package. This allows you to inherit from the ``SmartContract`` class and provides you access to useful functionality such as sending funds, hashing and saving data. If you're not using the Visual Studio template, you can create smart contracts just by including the ``Stratis.SmartContracts`` NuGet package in your project and inheriting from ``SmartContract`` as done here.

The only libraries that are allowed to be included in the current iteration of Stratis Smart Contracts are ``System``, ``System.Linq`` and ``Stratis.SmartContracts``.

::

  public Address Owner
  {
    get
    {
        return PersistentState.GetObject<Address>("Owner");
    }
    set
    {
        PersistentState.SetObject<Address>("Owner", value);
    }
  }

The Auction object has several properties structured similarly to the above. To persist data inside of a smart contract, we use ``PersistentState``. This can happen anywhere and doesn't have to occur within a property, but doing so inside properties like this makes the code inside your methods easier to read.

The ``Address`` class is included as part of ``Stratis.SmartContracts`` and is an abstraction over strings that enables the sending of funds to addresses as we'll see soon.

::

  public ISmartContractMapping<ulong> ReturnBalances
  {
    get
    {
        return PersistentState.GetMapping<ulong>("ReturnBalances");
    }
  }

``PersistentState.GetMapping`` returns a dictionary-like structure for storage. Using a standard .NET dictionary would require serializing and deserializing all of the dictionary data every time it was updated. For dictionaries with thousands of entries (think balances of a token contract), this would require a significant amount of processing. ``ISmartContractMapping`` instead stores individual values directly into the underlying ``PersistentState``.

For lists of information, developers also have access to ``ISmartContractList`` and ``PersistentState.GetList``.


::

  public Auction(ISmartContractState smartContractState, ulong durationBlocks)
   : base(smartContractState)
   {
       Owner = Message.Sender;
       EndBlock = Block.Number + durationBlocks;
       HasEnded = false;
   }

FROM HERE: FINISH UP CODE EXPLANATIONS, VALIDATE AND COMPILE THE CONTRACT WITH COMMAND-LINE TOOL.
