module AddressDecoder (

    input wire B2,

    input wire B1,

    input wire B0,

    

    input wire A15, A14, A13, A12, A11, A10, A9, A8, A7, A6, A5,

    

    output wire n_ROM_CS,

    output wire n_RAM_CS,

    output wire n_VIDEO_CS,

    output wire n_EXT0_CS,

    output wire n_EXT1_CS,

    output wire n_SID_CS,

    output wire n_SERIAL_CS,

    

    output wire GPIO_CS

);





    wire is_bank_0 = (!B2 && !B1 && !B0);

    

    // Check if address is in the $7E00 - $7FFF range

    wire is_io_page = (A15==0 && A14==1 && A13==1 && A12==1 && A11==1 && A10==1 && A9==1);

    

    wire is_io_active = (is_bank_0 && is_io_page);



    // --- MAIN CHIP SELECTS ---



    // ROM is top 32KB of Bank 0 ($8000 - $FFFF)

    assign n_ROM_CS = !(is_bank_0 && A15 == 1);

    

    // RAM is active in Banks 01-07 ALWAYS.

    // RAM is active in Bank 0 ONLY IF it is not ROM (A15=0) and not I/O (!is_io_page).

    assign n_RAM_CS = !( (!is_bank_0) || (is_bank_0 && A15 == 0 && !is_io_page) );





    // --- I/O CHIP SELECTS ---



    // V9958 Video: $7E00 - $7EFF (A8 = 0)

    assign n_VIDEO_CS = !(is_io_active && A8 == 0);

    

    // EXT0: $7F00 - $7F3F (A8 = 1, A7 = 0, A6 = 0)

    assign n_EXT0_CS = !(is_io_active && A8 == 1 && A7 == 0 && A6 == 0);

    

    // EXT1: $7F40 - $7F7F (A8 = 1, A7 = 0, A6 = 1)

    assign n_EXT1_CS = !(is_io_active && A8 == 1 && A7 == 0 && A6 == 1);

    

    // SID: $7F80 - $7F9F (A8 = 1, A7 = 1, A6 = 0, A5 = 0)

    assign n_SID_CS = !(is_io_active && A8 == 1 && A7 == 1 && A6 == 0 && A5 == 0);

    

    // GPIO (VIAs): $7FA0 - $7FBF (A8 = 1, A7 = 1, A6 = 0, A5 = 1)

    // NOTE: This is ACTIVE HIGH because it connects to CS1 on the W65C22.

    assign GPIO_CS = (is_io_active && A8 == 1 && A7 == 1 && A6 == 0 && A5 == 1);

    

    // SerialCs

    assign n_SERIAL_CS = !(is_io_active && A8 == 1 && A7 == 1 && A6 == 1 && A5 == 0);



endmodule
