Memory system design for the Microprocessor (Internal and External)

We covered Internal Memory in [](10%20-%20Architecture,%20Registers%20and%20Instructions.md#Memory%20Interface)

External memory is a bit more complicated and i'm not sure what's required in the exam, so check the book yourself: [](../books/3%20-%20Microprocessor%20and%20Microcontroller/Mazidi,%20Muhammad%20Ali_McKinlay,%20Rolin%20D_Mazidi,%20Janice%20Gillispie%20-%20The%208051%20Microcontroller_%20A%20Systems%20Approach%20(2013).pdf#page=395)

Just know that:

- EA = 0: all addresses are assumed external (ROM)
- EA = 1: only addresses after on-chip addresses are presumed external
- Need to use MOVC and MOVX for external addresses

> [!Note] The difference between MOVC and MOVX is that MOVC uses the program counter to access the external memory, while MOVX uses the data pointer.
> MOVC is used for code memory, while MOVX is used for data memory.
