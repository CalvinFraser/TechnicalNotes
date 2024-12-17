C++ is a compiled language and as such it has several steps before an output file (usually an executable or a library) is generated. This process is usually referred to as *building* or *translating*

These are usually defined as *Compilation* and *Linking*. With *Compilation* being further broken down into  **Pre-processing**, **Analysis** and **Codegen**

Let us look into each of these. 

## Compilation Process
In this stage each source file is processed separately. 
### Pre-processor 
The pre-processor handles a few key features:
- Header inclusion (Anything done using  `#Include` )
- Conditional Compilation (`#ifndef `)
- Macro substitution (`#DEFINE `)

The output of the pre-processor is a **Translation Unit**. These usually have a *.i* extension (Usually for intermediary). These units will include all of the header file content as well as the source (cpp) file content in a single file. 

### Analysis and Codegen 
The compiler analyses each translation unit and compiles the output into an **Object file**(s) or **object module**. These will typically have a *.o* or *.obj* extension. 

Inside an Object file there is typically data (The actual code and data that will be used in the resulting library/executable file). There is also meta-data which is additional information that a linker will use to combine the object files into a single output. 

This meta-data is generally symbols and values for:
- Function and object names (And their associated addresses):
	- Some names represent definitions (`defs`)
	- Other names represent references (`refs')
- Program section names 
	- 'text' or 'code' for machine instructions
	- 'literal' for initialised read-only data
	- 'data' for initialised read-write data
	- 'bss' for uninitialized read-write data

In general these object files are only "*mostly executable*" as in a lot of cases an instruction will contain an *external reference* to an entity defines in another translation unit (or object file). In those cases, the code generator fills that reference in with a placeholder (Usually one that can be easily found by the **linker**) . In addition, these object files are designed to be relocatable (That is the addresses may be moved). 
## Linking
The role of the **Linker** is to resolve the external references these object files may have by replacing them to addresses of the appropriate definitions. Often by combining object files or libraries of already compiled/built objects. It may also do other things such as relocation and the like of values and code. 

### One Definition Rule 
As a rule of thumb, any entity that a program uses must also be defined (In some way) by that same program. The C++ standard foramlizes this idea using the **One Definition Rule (ODR)** which states that *no translation unit may define certain entities more than once*. The term *"odr-used"* which spells out what uses of an entity require a program define the entity *exactly* once. 
![[Pasted image 20240801155037.png]]

If the linker is unable to find a definition for a reference within all of the available object files it will throw an **undefined reference** error. On the other side, if the linker finds *more than one definition* of a reference then it will throw an **multiple definitions** error. 

Some Entities must be defined in *exactly one translation unit*: 
- Non-inline Variables 
- Non-inline, non-template functions
	- **constexpr** and **consteval** functions are implicitly inline 

Other entities have a different set of rules:
- Class and enumeration types
- Inline variables
- Inline functions
- function templates
- Class templates and their members
- Partial template specialisations 

These can be defined in *more than one translation unit if* :
- Every definition must be in a different translation unit
- All definitions must be identical (Same sequence of tokens)

Usually the requirements to do this can be met by:
- Defining the entity in a single header that can be included by itself (Doesn't depend on other headers)

If those requirements are met then they program will be built as if there is only one definition for that entity.

### Linking Header Entities
The Standard does not specify how the compiler and linker should resolve symbols for entities that are usually defined in header  files. However, a common solution is for the compilation to generate slightly different meta-data for these entities so that the linker can identify and remove duplicate definitions. 

For example the Microsoft Visual C++ marks some of these with a "*pick any*" flag. Which tells the linker that it may pick any one of the duplicate definitions and discard the others. These Entities are sometimes called *"weak symbols"* .


## Templates 
Templates are typically defined within header files. 

#### Explicit Template Instantiation 
![[Pasted image 20240801163727.png]]
Used to allow for an explicit function declaration of a template function for a specific instance of its use. This means that it now behaves like a single defined entity. It must be declared in a header (With keyword extern) and must be defined in a source file without a provided body. Essentially telling the compiler and linker "When you expand this template, put this specific definition here. " 

### Memory Maps
When the linker creates the sections of a program (text, literal, data, bss, etc ) this is typically done for you. However, there is the option to control how the linker creates this memory map via *linker scripts*. This is alot more valuable when a platform has many memory spaces (ROM, RAM, etc.). 

Linker scripts typically work at an object file level. ![[Pasted image 20240801163401.png]]
This works fine for entities defined in source files (Non-inline, non-template functions, non-inline variables and explicitly specialized templates)
