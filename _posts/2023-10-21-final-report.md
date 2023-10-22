---
layout: post
title:  My Google Summer of Code Experience- A Journey of Learning, Growth, and Contribution
date: 2023-10-21 15:53:00-0400
description: Final Report
categories: 
giscus_comments: 
related_posts: 
toc:
  sidebar:
---

As I sit here on the eve of my Google Summer of Code project submission deadline, I can't help but reflect on the incredible journey I've been on over the past few months. When I first applied to GSoC, I was motivated by the opportunity to work on a real-world open source project and to gain valuable experience. I was also eager to learn from and collaborate with other talented developers from around the world.

I was thrilled to be accepted into the program and to be given the opportunity to work on the `Building an LLVM Backend for PRU` project under `Beagleboard.org`. 

Image

As I embarked on my Google Summer of Code project of Building an LLVM backend for PRU, I was filled with both excitement and apprehension. The prospect of working on such a widely used and respected compiler infrastructure was exhilarating, but the task of navigating the vast and unexplored territory of the LLVM codebase loomed large.

The initial stages of the project were indeed challenging. The LLVM codebase is immense, with thousands of lines of code spread across countless files and directories. Documentation, while present, was often sparse or outdated. As I delved deeper into the labyrinthine depths of the code, I found myself frequently lost. But I persevered, fueled by my determination to succeed and complete the project.

Gradually, I began to make progress. I started to understand the overall structure of the LLVM codebase and to identify the key components that would be involved in building a new backend. I also began to develop a deeper understanding of the various stages of the compilation process.

As the weeks turned into months, I slowly but surely made progress on the tasks. There were **three main milestones** of the project.


#### <ins>1. Understanding the Cpu0 backend tutorial and implementing it</ins>

The first milestone of the project was to understand and implement the Cpu0 backend tutorial. This tutorial provides a step-by-step guide to building a new LLVM backend. By following the tutorial, I was able to gain a deep understanding of the LLVM backend architecture.

The Cpu0 backend tutorial was originally developed for **LLVM version 3.1**. Since then, the LLVM codebase has undergone a number of significant changes. As a result, the tutorial was not directly compatible with the newer LLVM versions.

In order to use the tutorial as a starting point for my project, I had to make a number of modifications to the code.

You can find the steps of building the Cpu0 backend in [this blog](https://khushi-balia.github.io/blog/2023/week-2/) of mine.

#### <ins>2. New backend initialization for PRU<ins>

The second milestone of the project was to implement the necessary infrastructure for supporting the PRU target. This involved registering PRU as a target with **LLVM version 16** (the latest version)

One of the key tasks involved in registering PRU as a target was to create a new Target class for PRU. This class is responsible for providing LLVM with information about the PRU architecture, such as the instruction set, register set and so on.

Directory structure for PRU LLVM backend for **milestone 2**.

```
lib/TargetPRU/                                                                                                
├── CMakeLists.txt
├── MCTargetDesc/
│   ├── PRUInstPrinter.cpp
│   ├── PRUMCAsmInfo.cpp
│   ├── PRUMCCodeEmitter.cpp
│   ├── PRUMCCodeEmitter.h
│   ├── PRUMCTargetDesc.cpp
│   ├── PRUMCTargetDesc.h
│   └── PRUTargetStreamer.cpp
│   └── PRUTargetStreamer.h
├── TargetInfo/
│   ├── PRUTargetInfo.cpp
│   ├── PRUTargetInfo.h
│   └── CMakeLists.txt
├── PRU.h
├── PRU.td
├── PRUAsmPrinter.cpp
├── PRUCallingConv.td
├── PRUFrameLowering.cpp
├── PRUFrameLowering.h
├── PRUISelDAGToDAG.cpp
├── PRUISelLowering.cpp
├── PRUISelLowering.h
├── PRUInstrFormats.td
├── PRUInstrInfo.cpp
├── PRUInstrInfo.h
├── PRUInstrInfo.td
├── PRUMachineFunctionInfo.cpp
├── PRUMachineFunctionInfo.h
├── PRURegisterInfo.cpp
├── PRURegisterInfo.h
├── PRURegisterInfo.td
├── PRUSelectionDAGInfo.h
├── PRUSubtarget.cpp
├── PRUSubtarget.h
└── PRUTargetMachine.cpp
└── PRUTargetMachine.h
```
By implementing dummy code for the above, I was able to successfully register PRU as a target with LLVM version 16. This was a critical step in the development of the new PRU backend.

Image 

#### <ins>3. Adding code for PRU assembly code generation<ins>

The third and final milestone of the project was to add code to the backend for PRU assembly code generation. Originally, the plan was to implement this milestone using LLVM version 16. However, due to the very limited to no documentation available for the changes in LLVM 16, it was decided to implement it in LLVM 8 instead.

This decision ultimately proved to be the right one. By using LLVM 8, I was able to take advantage of the well-documented resources, which made it easier to develop a functional backend for the PRU architecture.

Directory structure for PRU LLVM backend for **milestone 3**, added files to the milestone 2 codebase.

```
lib/Target/PRU/
├── CMakeLists.txt
├── InstPrinter/
│   ├── CMakeLists.txt
│   ├── PRUInstPrinter.cpp
│   └── PRUInstPrinter.h
├── LLVMBuild.txt
├── MCTargetDesc/
│   ├── CMakeLists.txt
│   ├── PRUAsmBackend.cpp
│   ├── PRUELFObjectWriter.cpp
│   ├── PRUFixupKinds.h
│   ├── PRUMCAsmInfo.cpp
│   ├── PRUMCAsmInfo.h
│   ├── PRUMCCodeEmitter.cpp
│   ├── PRUMCTargetDesc.cpp
│   └── PRUMCTargetDesc.h
├── PRU.h
├── PRU.td
├── PRUAsmPrinter.cpp
├── PRUCallingConv.td
├── PRUFrameLowering.cpp
├── PRUFrameLowering.h
├── PRUISelDAGToDAG.cpp
├── PRUISelLowering.cpp
├── PRUISelLowering.h
├── PRUInstrFormats.td
├── PRUInstrInfo.cpp
├── PRUInstrInfo.h
├── PRUInstrInfo.td
├── PRUMCInstLower.cpp
├── PRUMCInstLower.h
├── PRUMachineFunctionInfo.cpp
├── PRUMachineFunctionInfo.h
├── PRURegisterInfo.cpp
├── PRURegisterInfo.h
├── PRURegisterInfo.td
├── PRUSubtarget.cpp
├── PRUSubtarget.h
└── PRUTargetMachine.cpp
└── PRUTargetMachine.h
```

Image

Workflow for Milestone 2 and Milestone 3:

##### <ins>TableGen description files<ins>

In order to implement a backend, I needed to write description files related to the target, that are called .td files. These .td files are translated into C++ source code by TableGen's tool llvm-tblgen when building the compiler. The generated code is then used in the backend implementation.

After the .td file is processed, some .inc files are generated under lib/Target/PRU/ in the build path. These .inc files are included in the backend implementation. The generated specification is what I specified in CMakeLists.txt.

There are multiple .td files, divided into categories to describe various information of the target platform, such as register information, instruction information, calling convention etc.

In order to implement a backend, I needed to write description files related to the target, that are called .td files. These .td files are translated into C++ source code by TableGen's tool llvm-tblgen when building the compiler. The generated code is then used in the backend implementation.

After the .td file is processed, some .inc files are generated under lib/Target/PRU/ in the build path. These .inc files are included in the backend implementation. The generated specification is what I specified in CMakeLists.txt.

There are multiple .td files, divided into categories to describe various information of the target platform, such as register information, instruction information, calling convention etc.

- **PRU.td** - This is the top level entry point for the PRU target

This file currently contains several other .td files, and then defines a subclass PRU based on the Target class.

- **PRURegisterInfo.td** - Register Information

All registers and register sets (RegisterClass) are defined in this file. The class PRUReg inherits from the class RegisterWithSubRegs.

```
class PRUReg<bits<16> num, string name, list<Register> subregs = [], list<string> altNames = []> : RegisterWithSubRegs<name, subregs>  
```   

- **PRUCallingConv.td** - Calling Conventions

The calling convention definitions describe the part of the ABI which controls how data moves between function calls.

- **PRUInstrFormats.td** - PRU Instruction formats

This file describes the patterns for definitions of target-specific instructions. The highest-level class InstPRU of the PRU instruction inherits from the Instruction class. 

```
class InstPRU<dag outs, dag ins, string asmstr, list<dag> pattern>
	: Instruction { … }
```

In addition, because the instructions of PRU are divided into more categories, those subcategories inherit from the InstPRU class.

```
class ALU_Inst_RR<bits<3> opcode, bits<4> instopcode, dag outs, dag ins, string asmstr,
              	list<dag> pattern>
	: InstPRU<outs, ins, asmstr, pattern> { … }
```

- **PRUInstrInfo.td** - Complete PRU Instruction Definitions 

The complete instruction definitions inherit from the instruction format classes to complete the TableGen Instruction base class. The multiclass functionality makes it easier to define multiple instructions that are very similar to each other. For example, the register-register (rr) and register-immediate (ri) ALU instructions are defined within the multiclass. It also contains some SDNode node definitions as well as operand type definitions.

```
def PRUcallseq_start :
             	SDNode<"ISD::CALLSEQ_START", SDT_PRUCallSeqStart,
                    	[SDNPHasChain, SDNPOutGlue]>;
```

#### <ins>Target registration<ins>

Once the necessary TableGen description files were written, the next step was to register the new target PRU with LLVM. The following things were done: 

- I created a **PRU.h** file under the PRU path. This file includes a number of other header files that are needed by the PRU backend.

- I then created the **PRUTargetMachine.cpp** file and the corresponding header file. The PRUTargetMachine class is responsible for providing LLVM with information about the PRU target. It has the `LLVMInitializePRUTarget()` function.

```
extern "C"  void LLVMInitializePRUTarget() {
    RegisterTargetMachine<PRUTargetMachine> X(getThePRUTarget());
}
```

- I then created a subdirectory **lib/Target/PRU/TargetInfo/**, and created PRUTargetInfo.cpp under this path. In this file, I called the RegisterTarget interface to register my PRU target.

```
        extern "C" void LLVMInitializePRUTargetInfo() {
       RegisterTarget<Triple::pru> X(getThePRUTarget(), "pru", "PRU microcontroller");
         }
```
         
- I also created a subdirectory **lib/Target/PRU/MCTargetDesc/**, and created a new MCTargetDesc.cpp file and its corresponding header file in this path. Here, a function `LLVMInitializePRUTargetMC()` is written. The MCTargetDesc class is responsible for providing LLVM with information about the PRU target's machine code.

```
extern "C" void LLVMInitializePRUTargetMC() {     
    RegisterMCAsmInfo X(getThePRUTarget(), createPRUMCAsmInfo);  TargetRegistry::RegisterMCInstrInfo(getThePRUTarget(), createPRUMCInstrInfo);  TargetRegistry::RegisterMCRegInfo(getThePRUTarget(), createPRUMCRegisterInfo);  TargetRegistry::RegisterMCAsmBackend(getThePRUTarget(), createPRUAsmBackend);  TargetRegistry::RegisterMCCodeEmitter(getThePRUTarget(), createPRUMCCodeEmitter);  TargetRegistry::RegisterMCInstPrinter(getThePRUTarget(), createPRUMCInstPrinter);
}
```

The PRUTargetInfo class calls the RegisterTarget method to register the PRU target with LLVM. The RegisterTarget method is defined in the TargetInfo class.
The MCTargetDesc class provides LLVM with information about the PRU target's machine code. This information is used by LLVM to generate code for the PRU target.
These were the main things done for milestone 2.

The work for milestone 3 was divided into 3 tasks:

#### <ins>1. Laying the Foundations - PRU Target Machine Architecture<ins>

In this task, I created/ modified a number of files to implement the target machine architecture for PRU. These files include:

- **PRUTargetMachine.h/.cpp**: These files define the essential class for the target machine architecture: PRUTargetMachine. This class inherits from LLVMTargetMachine and handles initialization tasks like DataLayout, relocation mod. The pivotal `getSubtargetImpl()` function constructs a Subtarget object, setting the stage for further architectural components.

- **PRUFrameLowering.h/.cpp**: This section focuses on Frame Lowering, critical for stack management. The PRUFrameLowering class inherits from TargetFrameLowering. It deals with the stack and manages various contents, including function parameters, registers, and more. The `hasFP()` method helps identify the presence of a Frame Pointer.

- **PRUInstrInfo.h/.cpp**: These files handle instruction-related actions, building upon the instruction descriptions generated by tablegen. The PRUInstrInfo class, inherited from PRUGenInstrInfo, contains critical elements, including the Subtarget object, which is initialised in the constructor.

- **PRUISelLowering.h/.cpp**: This file defines the interfaces that PRU uses to lower LLVM code into a selection DAG. Here, the PRUTargetLowering class is defined and inherited from TargetLowering. It integrates the PRUGenCallingConv.inc file, generated from PRUCallingConv.td, and provides methods like `LowerGlobalAddress()`,`LowerReturn()`, `LowerFormalArguments()` - transforms physical registers into virtual registers

- **PRUMachineFunctionInfo.h/.cpp**: Handling actions related to functions, the PRUMachineFunctionInfo class inherits the MachineFunctionInfo class. It declares methods related to parameters, currently serving as placeholders.

- **PRURegisterInfo.h/.cpp**: These files contain the PRUGenRegisterInfo.inc file, which defines the PRURegisterInfo based on PRUGenRegisterInfo. Several register-related methods are defined.

- **PRUSubtarget.h/.cpp**: This class, inherited from PRUGenSubtargetInfo, defines the PRUSubtarget. It maintains properties and establishes interfaces with other classes like `getInstrInfo()` and `getRegisterInfo()`

Completing Task 1 involved setting the stage for the entire PRU target machine architecture, from instruction handling to function management. These files form the cornerstone upon which subsequent development tasks will build.

#### <ins>2. Adding AsmPrinter Support<ins>

With the groundwork laid, I added AsmPrinter support, a crucial component in code generation. This task focuses on turning Machine DAGs into assembly code through the following steps:

- **InstPrinter/PRUInstPrinter.h/.cpp**: These files introduce the InstPrinter module, primarily responsible for outputting MCInst to assembly files. The PRUInstPrinter class inherits from MCInstPrinter and includes the crucial `printInstruction()` function, generated by tblgen based on PRUInstrInfo.td. Additionally, `getRegisterName()` and various internal functions aid in outputting instructions.

- **PRUMCInstLower.h/.cpp**: These files handle the lowering of MI (Machine Instruction) to MCInst (Machine Code Instruction). The PRUMCInstLower class is defined, with its primary member function, `Lower()`, converting MI to MCInst. This step involves setting the Opcode and Operand list.

- **MCTargetDesc/PRUMCAsmInfo.h/.cpp**: These files introduce the PRUMCAsmInfo class, inherited from MCAsmInfoELF. Although relatively simple, it defines some assembly file format specifics.

- **PRUAsmPrinter.h/.cpp**: The PRUAsmPrinter class serves as the entry point for outputting MI-structured programs to assembly files. This class is inherited from AsmPrinter and implements various `Emit` functions for emitting different content. The AsmPrinter differs from the InstPrinter in that it emits MI to a file and handles additional information like debugging and file descriptions.

Task 2 enriches the PRU target machine architecture by enabling it to produce assembly output, a fundamental step in the code generation process.

#### <ins>3. Implementing DAGToDAGISel - Bridging LLVM IR to Machine DAG<ins>

Task 3 marks a pivotal phase in developing the PRU target machine architecture. It focuses on extending support for DAGToDAGISel, a critical component responsible for converting LLVM IR DAG (Directed Acyclic Graph) to the Machine DAG. This process is essential for ensuring that the high-level LLVM IR code can be transformed into low-level machine code instructions.

- **PRUISelDAGToDAG.cpp**: This file plays a central role in the implementation of DAGToDAGISel for the PRU architecture. The `PRUDAGToDAGISel` class is defined here, inheriting from the `SelectionDAGISel` class. It provides several key interfaces and functions:

  - *Select*: The Select function serves as the entry point for instruction selection. It is used for initial attempts at selecting instructions.

  - *SelectCode*: If the initial selection in the Select function doesn't result in a valid instruction, the process proceeds to the SelectCode function. The SelectCode function is defined in the `PRUGenDAGISel.inc` file, generated by tablegen. It attempts to select a valid instruction based on the patterns specified in the target description (td) files.

  - *SelectAddr*: The SelectAddr function is responsible for selecting the execution of address modes. Since some nodes in the IR DAG represent complex address operands, this function handles their selection. The target description (td) files can describe specific patterns for address operands, and these patterns are left for the SelectAddr function to process.

- **PRUTargetMachine.cpp**: In this file, a directive selector for DAG selection is registered. The PRUSEISelDAG class is added as part of this registration process. The `addInstSelector()` method, which overrides a method in the parent class `TargetPassConfig`, ensures that DAG instruction selection for PRU is supported.

Task 3 completes the critical chain that allows LLVM to translate high-level LLVM IR code into machine-level instructions. It encompasses instruction selection, handling complex address operands, and ensuring that the generated machine code adheres to the PRU target architecture.

### <ins>Project Details<ins>:

Project Name: **Building an LLVM Backend for PRU**

Mentors: **Vedant Paranjape**, **Abhishek Kumar**, and **Shreyas Atre**

Code: [llvm-project](https://github.com/Khushi-Balia/llvm-project/tree/llvm-pru)

Introductory Video: https://youtu.be/f4LVIW9VlrM?feature=shared

Proposal: [Khushi Balia - Building an LLVM Backend for PRU](https://elinux.org/BeagleBoard/GSoC/2023_Proposal/Khushi-Balia)

#### <ins>Conclusion<ins>
In conclusion, the journey of my Google Summer of Code project, "Building an LLVM Backend for PRU," has been nothing short of transformative. What began as an ambitious undertaking to contribute to the open-source community has culminated in the successful creation of a functional LLVM backend for PRU, capable of generating assembly code for essential PRU instructions.

Throughout this incredible journey, I have faced and overcome various challenges,from navigating the intricacies of the vast LLVM codebase to bridging the gap between LLVM IR and machine-level instructions.

I am very grateful to my mentors, **Vedant Paranjape**, **Abhishek Kumar**, and **Shreyas Atre**, whose unwavering guidance and support have been instrumental in this endeavor. Their expertise, encouragement, and commitment to my growth as a developer have played a pivotal role in the project's success

I am thankful for the opportunity to participate in Google Summer of Code and to work with BeagleBoard.org on this project. This experience has not only been a journey of learning and growth but also a testament to the power of collaboration and open-source development. I am excited to continue contributing to the community and building on the foundation laid during GSoC.