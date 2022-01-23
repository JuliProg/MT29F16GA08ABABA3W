![Create new chip](https://github.com/JuliProg/MT29F16GA08ABABA3W/workflows/Create%20new%20chip/badge.svg?event=repository_dispatch)
![ChipUpdate](https://github.com/JuliProg/MT29F16GA08ABABA3W/workflows/ChipUpdate/badge.svg)
# Join the development of the project ([list of tasks](https://github.com/users/JuliProg/projects/1))


# MT29F16GA08ABABA3W
Implementation of the MT29F16GA08ABABA3W chip for the JuliProg programmer

Dependency injection, DI based on MEF framework is used to connect the chip to the programmer.

<section class = "listing">

# Chip parameters

```c#


        //--------------Warning!!!!!!!!!!!!!!!-------------------------------------------
        //
        //   The first operation must be a command
        //   Reset  !!!
        //
        //--------------------Vendor Specific Pin configuration---------------------------

        //  VSP1(38pin) - NC    
        //  VSP2(35pin) - NC
        //  VSP3(20pin) - NC

        ChipAssembly()
        {
            myChip.devManuf = "Micron";
            myChip.name = "MT29F16GA08ABABA3W";
            myChip.chipID = "2C48002689000000";      // device ID - 2Ch 48h 00h 26h 89h 00h 00h 00h 

            myChip.width = Organization.x8;    // chip width - 8 bit
            myChip.bytesPP = 4096;             // page size - 4096  byte (4Kb)
            myChip.spareBytesPP = 224;          // size Spare Area - 224 byte
            myChip.pagesPB = 128;               // the number of pages per block - 128 
            myChip.bloksPLUN = 4096;           // number of blocks in CE - 4096 
            myChip.LUNs = 1;                   // the amount of CE in the chip
            myChip.colAdrCycles = 2;           // cycles for column addressing
            myChip.rowAdrCycles = 3;           // cycles for row addressing 
            myChip.vcc = Vcc.v3_3;             // supply voltage
            (myChip as ChipPrototype_v1).EccBits = 1;                // required Ecc bits for each 512 bytes

```
# Chip operations

```c#


            //------- Add chip operations    https://github.com/JuliProg/Wiki#command-set----------------------------------------------------

            myChip.Operations("Reset_FFh").
                   Operations("Erase_60h_D0h").
                   Operations("Read_00h_30h").
                   Operations("PageProgram_80h_10h");

```
# Chip registers (optional)

```c#


            //------- Add chip registers (optional)----------------------------------------------------

            myChip.registers.Add(                   // https://github.com/JuliProg/Wiki/wiki/StatusRegister
                "Status Register").
                Size(1).
                Operations("ReadStatus_70h").
                Interpretation("SR_Interpreted").
                UseAsStatusRegister();



            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/ID-Register
                "Id Register").
                Size(8).
                Operations("ReadId_90h");
            //Interpretation(ID_interpreting);

            myChip.registers.Add(                  // https://github.com/JuliProg/Wiki/wiki/OTP
                "OTP memory area").
                Size((4096+224)*128).
                Operations("OTP_Mode_On_v1").      // set chip to OTP mode then Read or Programm block[0]
                Operations("OTP_Mode_Off_v1");     // set chip to normall mode


            myChip.registers.Add(
                "Unique Id").
                Size(32).
                Operations("ReadUniqueId_EDh");

            myChip.registers.Add(
               "Parameter Page (ONFI parameter)").
               Size(768).
               Operations("ReadParameterPage_ECh");


```
# Interpretation of ID-register values ​​(optional)

```c#


        public string ID_interpreting(Register register)   
        
```
</section>






















