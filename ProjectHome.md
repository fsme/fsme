# Intro #
**Finite State Machine** is well known and well studied concept which is taught in colleges and is widely applied long ago in the industries. But despite immediate relationship of FSM with computer sciences in programming this concept is used very superficially.

For example, any of existing [programming styles](http://en.wikipedia.org/wiki/Programming_paradigm) or languages does not imply dependence on FSM model. Applicants, [such like this](http://en.wikipedia.org/wiki/Automata-based_programming), do not cope with the task and do not deserve the names assigned to them. Once upon a time, I saw a recoder of text which included 256 `'case'` operators. Sorry, guys, but it not a paradigm, and it is not programming even.

This project offers a **new programming paradigm**, _BEnding Engine Programming_, i.e. **BEEP**. The **specific program structure** for the paradigm implementation **is offered**.

# FSM Representation #

## Table Representation ##
As we know, a two-dimensional tables can use for representations of FSM. Rows and columns represent _Events_ and _States_, and cells contain _Actions_ and _Transitions_.

|**Event/State**| disabled |  wait  | read | write |  overflow |
|:--------------|:---------|:-------|:-----|:------|:----------|
|    `ON` | state(wait)  |   |   |    |  |
|    `INPUT` |   |  state(read) | get() |        |   |
|    `SEEK`  |  | seek()  |  seek() | seek()  | clear()  |
|    `OFF`  |   | state(disable)  | state(disable)  | state(disable) | state(disable)  |

Each State may be represented as vector containing pointers to handlers (Circuits) of the Event at this State.

| **read=>** | empty() | get() | seek() |   state(disable)  |
|:-----------|:--------|:------|:-------|:------------------|


| ON EventID=0 |     INPUT EventID=1  | SEEK EventID=2 | OFF EventID=3    |
|:-------------|:---------------------|:---------------|:-----------------|
| empty() | get() | empty() |   state(disable)  |

Such **vector** exists in any computer, a.k.a an _Interrupt Vector Table_. However, in difference from **FSME**, it is changes a little and has no necessary environment and communications.

## Filesystem Reflection ##
tblfs

# Events is used for Control Statement #
The Event (content or identification number) is used as index at list of pointers to handlers for automatic call of its handler.

> ## Example ##
The code of automate implementing that architecture will take only few lines in any programming language.

```
// simple example, ANSI C

for(;;) {
	if ( (EventID = get_event()) < 0 ) pause();
	else
            (*handler_ptr[EventID]) ();
}

```

A **Transition** is implemented by change pointers or change the all list.

Actually, is not present need of language support call to the pointer. We can use `goto`. Goto faster, Goto shortly, **`Goto` rules**.

# Terminology #
New terminology is offered to prevent confusion of terms.

Instance of the _FSM engine_ is called the _**Bot**_. Handler of event called _**Unit**_. _Pointer_ to _**Unit**_ named _**Circuit**_. _List of **Circuits**_, which use for control statment, named _**State**_, which can change through _**Transition**_. Input of _**Bot**_ called _**Events**_.

And me you can call Pa or maybe Grapa (the paradigm of cause).

# FSM Engine #
And so, the Engine consists from

  1. Pointers to handlers of events, called _**Circuits**_
  1. List of _**Circuits**_, called _**State**_
  1. Input stream, called _**Events**_
  1. Use change _**Circuits**_ for _**Transition**_ to another _**State**_
  1. Waiting events and automatic call handlers of them

It is easy to implement such simple program even on an assembler or machine code, just for fun.
```
;------------------------------------
; The Bending Unit #23, Intel8086 assembler
;-------------------------------------
; BP = Base Address, begin of I/O stream of Events (256 bytes)
; SI = Current Input
; DI = Current Output
; CX =  Counter of Events
; BP+100h =  Offset to State (List of Circuits)

Rotate:	AND	SI,00FF	; Mask off begin of Events
	OR	SI,BP	; Mask on begin of Events
	LOOP	Next	; if Events.is_empty()
	HLT		; just sleepy
	JMP	Rotate	; 
Next:	CLD
	LODSB		; ------+ Got EVENT     
	XOR	BH,BH	;	|  Clean up junk
	MOV	BL,AL	;	|  Load EVENT to Index Register
	SHL	BX,1	;	|  2 bytes per pointer
	ADD	BX,BP	;	|  Offset from Base Address
	JMP	[BX+0100];	+-->>> Bending >>>

```

This is it. This is [deterministic finite state machine](http://en.wikipedia.org/wiki/Deterministic_finite_state_machine).

It is the **common** _(general, universal)_ **code** for all the bots **of FSME**, all the programs use this code for input data and control statement.

# Strict Verification Input Data #
Byte by byte processing at the model does strict verification of all input data.

The **State** is a Sieve, through which can pass the only strictly given sequence of bytes. The behavior of the program-bot is completely controlled by the programmer. Perhaps rigorous mathematical proof of impossibility of casual behavior. To get weird results by simply entering a random sequence of bytes is impossible.

Hacking software as it is practiced in existing regular systems excluded.

# Grey Goo #
But for real programming a one machine is not enough. Any complex task to inflate size of the engine to skyscraper. FSM with quantity of states and events at thousands will be very difficult for building and debuging. This problems which non-deterministic automate are solving.

As always the decision already is, clever people everything invented for us long ago. I meen replicator by Drexler or the fork of UNIX process. The _FSM engine_ shall be able to breed and to work as a swarm.

# IBC: Inter-Bot Communication #
At _cloning_ between parent and child secure ciphering communication is created automatically.

#### Type A: Ring ####
  1. Output stream of parent is addressed on input stream buffer of child
  1. Output stream of child is addressed on input stream of parent

These two or set of bots form a crew. Between them is created bidirectional pipes.

#### Type B: Synapse ####
  1. In/Out stream of parent is addressed on Out/In of child
  1. Out/In stream of child is addressed on In/Out to another _Bot_

This opportunity to connect to another gang. Synapse may be _Input_, _Output_ or _Bidirectional_.

#### Type C: Saint ####
> The child is release to independent life. Sometime somewhere it may join to a _Ring_ or could becomes a _Synapse_, if carries.

A Bot may have any quantity of  I/O buffers any direction, in or out. All of them are used for **IBC** by the specified ways.

Data streams are ciphered by default by a private key which is generated and copied in case of _cloning_.

# Purposes, plans and status #
Purposes of this project:
  * Invent and testing data structures and algorithms for the **BEEP** paradigm
  * Progress of skills assembler and hex x86 coding

The researches is carried for use in the [Meta OS](http://code.google.com/p/meta-os/) and [Metacomputer](http://code.google.com/p/metacomputer/) projects.

The source code is not present and is not planned yet, cause don't using any compiler. You can download only [binary files](http://code.google.com/p/fsme/source/browse/#git%2Ffiles):
  1. ` fsmeXXXX.com ` - version for MS-DOS (XXXX - platform, make at debug.exe)
  1. ` fsme.bot ` - (planned) bootable HDD/USB image (for native or virtual machine)

First of all will be developed simple monitor and debugger. It must be able assemble and disassemble x86 codes and Events codes, cause Events use as second level language. Support of line-by-line comments is also necessary.

Read [Wiki pages](https://code.google.com/p/fsme/w/list) for more.

I will do it, here we go!