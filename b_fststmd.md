# Project File Structure

Source code is organized into categories annotated by the first letter of each file. This information is stated at the top of `src/m_main.c` file with exhausive list of source file in each category along with a brief detail.

All categories are listed here.

1. `c` - Compiler
2. `d` - Process designer
3. `e` - Code editor
4. `f` - Files (saved games are not currently implemented)
5. `g` - Game
6. `h` - Story
7. `i` - Main interface
8. `m` - Main/Miscellaneous
9. `p` - Panels (the windows that appear on the right of the screen)
10. `s` - Start (runs the start menu and some other things that actually belong in the `g_` files)
11. `t` - Templates (not C++ templates, but templates for panel, template menu, etc)
12. `v` - Virtual machine
13. `x` - Sound effect
14. `z` - Shape editor

List of files according to its category along with brief detail (copied from comment as seen at top of `src/m_main.c`)

## Main/miscellaneous

`m_input.c` - contains Allegro calls that read user input (see also `i_input.c`)
`m_main.c` - this file. Contains the main function and some initialisation stuff
`m_maths.c` - special maths functions

`m_config.h` - header file containing some configuration options
`m_globvars.h` - a few global variable extern declarations

## Game

`g_cloud.c` - handles clouds (particle effects and similar)
`g_command.c` - user selection of processes, and giving of commands
`g_game.c` - contains the main game loop and some initialisation stuff
`g_group.c` - not currently used
`g_method.c` - code for executing object methods and class calls
`g_method_clob.c` - not currently used
`g_method_core.c` - core and component (member) methods
`g_method_misc.c` - some miscellaneous method code
`g_method_pr.c` - not currently used
`g_method_std.c` - standard methods
`g_method_sy.c` - not currently used
`g_method_uni.c` - some other standard methods that are internally treated as "universal" methods (this distinction is currently not relevant to gameplay)
`g_misc.c` - some miscellaneous game stuff
`g_motion.c` - handles game physics
`g_packet.c` - runs packets (bullets)
`g_proc.c` - deals with certain aspects of process creation and destruction
`g_proc_new.c` - deals with process creation
`g_proc_run.c` - runs processes
`g_shapes.c` - initialises the shape (process/component design and collision information) data structures
`g_world.c` - initialises and runs aspects of the game world
`g_world_back.c` - map background
`g_world_map.c` - map background
`g_world_map_2.c` - map background

`g_header.h` - contains a vast amount of game data declarations

## Story

> starts with h mostly because of its place in the alphabet

`h_interface.c` - story mode region selection interface
`h_mission.c` - sets up story missions
`h_story.c` - general story code

## Interface 

> runs the display

`i_background.c` - some background stuff. May not actually do anything at the moment.
`i_buttons.c` - contains code for interface buttons
`i_console.c` - runs consoles (the text boxes that appear on the game display) and a few other things
`i_disp_in.c` - contains some display initialisation code
`i_display.c` - runs the display
`i_error.c` - writes some errors to consoles. I don't think this is currently used.
`i_input.c` - some input functions that really should be integrated into `m_input.c`
`i_sysmenu.c` - mostly unused. sysmenu display is mostly in `p_draw.c` now, I think
`i_view.c` - contains some view initialisation stuff

`i_header.h` - display-related declarations

## Panels

> the windows that appear on the right of the screen

> the panel code in general is a horrible mess that needs to be rebuilt completely,
  but it mostly works.
> most of the code for the editor panel is in the `e_*.c` files.

`p_draw.c` - draws the panel display
`p_init.c` - initialises the panels
`p_panels.c` - general panl code

## Sound effects

`x_init.c` - initialises sound. Creates the separate sound thread.
`x_music.c` - music code
`x_sound.c` - sound effect code
`x_synth.c` - synthesiser

## Start

> runs the start menu and some other things that actually belong in the `g_*.c` files

`s_menu.c` - runs the start menu
`s_mission.c` - loads and starts missions
`s_turn.c` - not currently used

## Templates

`t_draw.c` - draws the template panel
`t_files.c` - opens and reads files for templates
`t_init.c` - initialises templates
`t_template.c` - handles templates and the Te template menu

## Virtual machine

`v_interp.c` - contains the bcode interpreter

## Compiler

`c_compile.c` - the compiler
`c_fix.c` - converts the #process header into a process design for a template
`c_generate.c` - generates bcode from the compiler's output
`c_init.c` - initialises the compiler
`c_keywords.c` - giant list of compiler keywords
`c_lexer.c` - the compiler's lexer
`c_prepr.c` - the minimal preprocessor

`c_header.h` - contains a lot of compiler-related stuff

## Process designer

`d_code.c` - the autocoder (generates code for user-designed processes)
`d_code_header.c` - generates the `#process` header from the contents of the design window
`d_design.c` - general design code
`d_draw.c` - draws the designer panel
`d_geo.c` - some designer geometry stuff

## Code editor
`e_clip.c` - clipboard (cut/paste, undo/redo etc)
`e_complete.c` - code completion
`e_editor.c` - general code for the editor
`e_files.c` - opening/closing files
`e_help.c` - help system
`e_inter.c` - editor interface stuff
`e_log.c` - the message log (used by other panels as well)
`e_slider.c` - code to run sliders/scrollbars (used by other panels as well)
`e_tools.c` - some random editor stuff

`e_header.h` - general editor declarations

## Files
> saved games are not currently implemented, so none of these files do much, if anything

`f_game.c` - gamefiles (no longer used)
`f_load.c` - loads and verifies saved games (not implemented)
`f_save.c` - saves games (not implemented)
`f_turn.c` - turnfiles (not implemented)

## Shape editor

`z_poly.c` - an editor for me to use to design components. Generates code
 to be pasted into `g_shapes.c`. It's not user-friendly and using it at all
	requires recompilation.
