# utl-replace-missing-values-with-constants-based-on-column-another-column
This method requries that all values are missing in set of sequential columns, val1-val7.
    %let pgm=utl-replace-missing-values-with-constants-based-on-another-column;

    Replace missing values with constants based on column another column

    This method requries that all values are missing in set of sequential columns, val1-val7.

    github
    https://tinyurl.com/yauv8mzz
    https://stackoverflow.com/questions/77026943/replace-all-missing-value-based-on-other-column-value

    StackOverflow
    https://stackoverflow.com/questions/77026943/replace-all-missing-value-based-on-other-column-value

    SOLUTIONS

           0 WPS/SAS (does not work in dropdown to wps use notepad++?)
             Uses peek and poke. Two of the most powerfull assembler instructions are load and store.
           1 wps no sql
           2 wps r
             https://stackoverflow.com/users/13321647/tarjae
           3 Related reps on end


    NOTES (SAS/WPS DIFFERENTIATORS)

       0 Not available in WPS.The best editor ever created, SAS DMS?
         Command macros allow all of the SAS language to interact with editor text.
         https://youtu.be/DzNdKj1CQps  (Point and shoot videos 1-5 videos)

       1 One of the few languages that support peek and poke (however usually
         turned off in baby SAS (lackdown sas (price not reduced))

       2 Seemless integration of local SAS and Server SAS (server sas code sandwhiched between local code -
         like IBM PLAS - assembler language sandwiched between 4 gen PL1 statements)

       3 Never available in WPS. In the 80s SAS supported 128 byte floats (maybe lokking into now?)

       4 Seemless Mutitple level language compilation (both pre datastep compilation and within datastep execution,

       5 Ability to map sas floats numeric data taype int oter data representations ( 3 byte to 8 byte floats..)
         (s370 paked decimal. Pos integer binary ..) Really need 128 bit floats - 9-16 byte floats.

       7 A little slower in WPS?. The fastest C comipler ever created. Makes looping in SAS vert very fast.

       8 WPS has better integration. SAS has always been very aware of integration with other languages. Has one of the fastes SQL
         compilers. Although has basically one data structure, this allows easier integration with other languages.

       9 Not tested in WPS. Handles big data better than other langualges. SAS can read 2986 GB on IBM 3390 volumes. Can depend on
         operating system limits. Often depends on operating system. IBM wins?



    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input
       id type $ val1 val2 val3 val4 val5 val6 val7;
    cards4;
    1 a 10 13 8 10 15 5 11
    2 b . . . . . . .
    3 a . . . . . . .
    4 c . . . . . . .
    5 b 35 39 38 25 44 24 42
    ;;;;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                     |                                                  */
    /* SD1.HAVE total obs=5 02JUN2022:09:57:59                             |                                                  */
    /*                                                                     |                                                  */
    /* INPUT                                                               | RULES                                            */
    /*                                                                     |                                                  */
    /*  ID    TYPE    VAL1    VAL2    VAL3    VAL4    VAL5    VAL6    VAL7 |                                                  */
    /*                                                                     |                                                  */
    /*   1     a       10      13       8      10      15       5      11  | For type a replace missing in v1-v7 with 12      */
    /*   2     b        .       .       .       .       .       .       .  | For type b replace missing in v1-v7 with 38      */
    /*   3     a        .       .       .       .       .       .       .  | For type c replace missing in v1-v7 with 50      */
    /*   4     c        .       .       .       .       .       .       .  |                                                  */
    /*   5     b       35      39      38      25      44      24      42  |                                                  */
    /*                                                                     |                                                  */
    /*                                                                     |                                                  */
    /* OUTPUT                                                              |                                                  */
    /*                                                                     |                                                  */
    /*   ID    TYPE    VAL1    VAL2    VAL3    VAL4    VAL5    VAL6    VAL7|                                                  */
    /*                                                                     |                                                  */
    /*    1     a       12      12      12      12      12      12      12 |                                                  */
    /*                                                                     |                                                  */
    /*    2     b       38      38      38      38      38      38      38 |  all 38 becase type=b                            */
    /*    3     a       12      12      12      12      12      12      12 |                                                  */
    /*    4     c       50      50      50      50      50      50      50 |                                                  */
    /*                                                                     |                                                  */
    /*    5     b       38      38      38      38      38      38      38 |                                                  */
    /*                                                                     |                                                  */
    /**************************************************************************************************************************/
    /*___                                _
     / _ \   ___  __ _ ___   _ __   ___ | | _____
    | | | | / __|/ _` / __| | `_ \ / _ \| |/ / _ \
    | |_| | \__ \ (_| \__ \ | |_) | (_) |   <  __/
     \___/  |___/\__,_|___/ | .__/ \___/|_|\_\___|
                            |_|
    */
    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    libname sd1 'd:/sd1';
    data sd1.want;
       set sd1.have;
       array slot[7] val1-val7;
       adrslot=addrlong(slot[1]);
       if      type='a' then call pokelong(repeat(put(12,rb8.),6),adrslot,96);
       else if type='b' then call pokelong(repeat(put(38,rb8.),6),adrslot,96);
       else if type='c' then call pokelong(repeat(put(50,rb8.),6),adrslot,96);
       drop adrslot;
    run;quit;

    /*---- Get the address od the first slot                                 ----*/
    /*---- Poke in the appropriate constant 7 times                          ----*/
    /*---- Once for each slot                                                ----*/

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* ID    TYPE    VAL1    VAL2    VAL3    VAL4    VAL5    VAL6    VAL7                                                     */
    /*                                                                                                                        */
    /*  1     a       10      13       8      10      15       5      11                                                      */
    /*  2     b       38      38      38      38      38      38      38                                                      */
    /*  3     a       12      12      12      12      12      12      12                                                      */
    /*  4     c       50      50      50      50      50      50      50                                                      */
    /*  5     b       35      39      38      25      44      24      42                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                                _
    / | __      ___ __  ___   _ __   ___    ___  __ _| |
    | | \ \ /\ / / `_ \/ __| | `_ \ / _ \  / __|/ _` | |
    | |  \ V  V /| |_) \__ \ | | | | (_) | \__ \ (_| | |
    |_|   \_/\_/ | .__/|___/ |_| |_|\___/  |___/\__, |_|
                 |_|                               |_|
    */
    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    libname sd1 "d:/sd1";

    data sd1.want;

     set sd1.have;

     array v1_7 val1-val7;

     do over v1_7;

       if      type='a' then v1_7 = coalesce(12,v1_7);
       else if type='b' then v1_7 = coalesce(38,v1_7);
       else if type='c' then v1_7 = coalesce(50,v1_7);

     end;

    run;quit;

    /*___
    |___ \  __      ___ __  ___   _ __
      __) | \ \ /\ / / `_ \/ __| | `__|
     / __/   \ V  V /| |_) \__ \ | |
    |_____|   \_/\_/ | .__/|___/ |_|
                     |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    libname sd1 "d:/sd1";

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(dplyr);
    want <- have %>%
      mutate(across(-c(1,2), \(x) {
        case_when(
          TYPE == "a" ~ coalesce(x, 12),
          TYPE == "b" ~ coalesce(x, 38),
          .default    = coalesce(x, 50)
        )
      }));
    endsubmit;
    import data=sd1.want r=want;
    run;quit;\
    ');

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* ID    TYPE    VAL1    VAL2    VAL3    VAL4    VAL5    VAL6    VAL7                                                     */
    /*                                                                                                                        */
    /*  1     a       10      13       8      10      15       5      11                                                      */
    /*  2     b       38      38      38      38      38      38      38                                                      */
    /*  3     a       12      12      12      12      12      12      12                                                      */
    /*  4     c       50      50      50      50      50      50      50                                                      */
    /*  5     b       35      39      38      25      44      24      42                                                      */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*        _       _           _
     _ __ ___| | __ _| |_ ___  __| |  _ __ ___ _ __   ___  ___
    | `__/ _ \ |/ _` | __/ _ \/ _` | | `__/ _ \ `_ \ / _ \/ __|
    | | |  __/ | (_| | ||  __/ (_| | | | |  __/ |_) | (_) \__ \
    |_|  \___|_|\__,_|\__\___|\__,_| |_|  \___| .__/ \___/|___/
                                              |_|
    */
    https://github.com/rogerjdeangelis/utl-setting-your-classic-editor-dms-function-keys-mouse-actions-and-command-line
    https://github.com/rogerjdeangelis/utl_classic_editor_command_macro_performance_package
    https://github.com/rogerjdeangelis/utl_classic_editor_hilite_and_evaluate_math_functions
    https://github.com/rogerjdeangelis/utl_classic_editor_keyboard_macros_and_abbreviations
    https://github.com/rogerjdeangelis/utl_classic_editor_part_II
    https://github.com/rogerjdeangelis/utl_classic_editor_pfkeys_prefix_mouse
    https://github.com/rogerjdeangelis/utl_classic_editor_place_comment_box_at_cursor_location_with_single_keystroke
    https://github.com/rogerjdeangelis/utl_classic_editor_programatic_cursor_positioning_with_store_cut_and_paste
    https://github.com/rogerjdeangelis/utl_classic_editor_seemless_connection_workstation_unix_server
    https://github.com/rogerjdeangelis/utl_classic_editor_using_a_prefix_mask_to_facilitate_fixed_column_data_entry
    https://github.com/rogerjdeangelis/utl_performance_package_SAS_classic_editor
    https://github.com/rogerjdeangelis/utl_sas_classic_editor
    https://github.com/rogerjdeangelis/Creating-command-macros-for-the_1980s-DMS-Classic-Editor
    https://github.com/rogerjdeangelis/utl-are-classic-editor-command-macros-fsview-and-fsedit-more-useful-than-viewtable
    https://github.com/rogerjdeangelis/utl-classic-editor-comment-out-a-block-of-text
    https://github.com/rogerjdeangelis/utl-classic-editor-commnand-macro-to-repair-sas-satement-with-uunbalanced-quotes
    https://github.com/rogerjdeangelis/utl-how-to-set-up-the-classic-1980s-classic-editor
    https://github.com/rogerjdeangelis/utl-latest-command-macros-for-the-sas-classic-editor
    https://github.com/rogerjdeangelis/utl-location-of-program-folder-and-classic-editor-program-versioning
    https://github.com/rogerjdeangelis/utl-sas-classic-editor-function-keys-for-comments
    https://github.com/rogerjdeangelis/utl-sas-scripting-commands-only-available-in-the_1980s-classic-editor
    https://github.com/rogerjdeangelis/utl-spell-check-words-in-any-classic-editor-window-ie-pgm-note-log-output
    https://github.com/rogerjdeangelis/utl-trimming-padded-lines-in-the-classic-editor
    https://github.com/rogerjdeangelis/utl_sas-classic-editor-point-and-shoot-a-proc-freq


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
