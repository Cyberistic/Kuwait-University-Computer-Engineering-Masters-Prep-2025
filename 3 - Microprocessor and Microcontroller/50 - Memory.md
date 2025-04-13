Memory system design for the Microprocessor (Internal and External)

We covered Internal Memory in [[10 - Architecture, Registers and Instructions#Memory Interface]]

External memory is a bit more complicated and i'm not sure what's required in the exam, so check the book yourself: [[Mazidi, Muhammad Ali_McKinlay, Rolin D_Mazidi, Janice Gillispie - The 8051 Microcontroller_ A Systems Approach (2013).pdf#page=395]]

Just know that:

- EA = 0: all addresses are assumed external (ROM)
- EA = 1: only addresses after on-chip addresses are presumed external
- Need to use MOVC and MOVX for external addresses

> [!Note] The difference between MOVC and MOVX is that MOVC uses the program counter to access the external memory, while MOVX uses the data pointer.
> MOVC is used for code memory, while MOVX is used for data memory.
