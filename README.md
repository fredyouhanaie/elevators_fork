Elevators Demo
==============

Elevator control system demonstrating Erlang/OTP upgrades.

About the recent updates
------------------------

> _The following notes will be moved into the pull request in due
  course._

The `update-1` branch aims to bring the project in line with the
current versions of the Erlang/OTP and the rest of the related
toolchain.

The main goal has been to get the simulator working with *minimal
changes* to the original source code. As will be noted in the updated
graphic interface!

So far, the update includes:

* addition of `gen_statem` calls to replace the deprecated `gen_fsm`
  behaviour. However, the `gen_fsm` calls have been left in and can be
  used through conditional compilation.
* The former `rebar` and `relx` tools have been replaced with `rebar3`.
* The calls to the deprecated `gs` (graphics system) module have been
  replaced with a mock module (`gs_log`) in order enable us to check
  the workings of the rest of the updates. Hopefully, a proper
  graphics interface will be added in the near future :)

For the graphic interface there are many options available, `wx`,
browser based, TUI, CLI, etc. It may be worth considering an elevator
graphics interface (buttons, doors, movement) abstraction layer in
order to enable any/all possible interfaces types.

### Quick start (update-1)

* Make sure you have `make` and `rebar3` installed and in your `$PATH`
* Type `make` to build everything
* Start the release with

> `./_build/default/rel/elevators/bin/elevators console`

* Make elevators move and notice they stop at each floor
  * There is no GUI, so you will need to use `gs_log`, and your
    imagination, instead :-(
  * `gs_log:fbutton(F)` generates the event for the call button on
     floor `F`.
  * `gs_log:ebutton(E, F)` generates the event for the floor `F`
    button in elevator `E`.
  * The type and range of `E` and `F` are not checked. In the real GUI
    you can only click on buttons with valid values.
  * Bad `E` will cause crash and restart of the elevator `scheduler`.
  * Bad `F` will cause an elevator to move indefinitely!
  * In the log output,
    * `{move,{0,10}}` indicates elevator moving down
    * `{move,{0,-10}}` indicates elevator moving up
    * It takes 8 moves in the same direction to move to the next floor.
    * `{fill,cyan}` indicates door opening.
    * `{fill,black}` indicates door closing.
    * when an elevator arrives at a floor, the door opens, and after
      one second it closes again.

*  (Exercise: Find bugs, fix them and build a release upgrade)
*  Place fixes in `upgrade/scheduler.erl`
*  Run `./v1to2.sh`

Note that `v1to2.sh` will build the releases in `_build/` and deploy
them in `_tmp/elevators/`. The `_build` and `_tmp/elevators/`
directories are deleted and recreated each time the script is run!

You can also use `rebar3 shell` to start the simulator and experiment
with it.

### Working with releases

* Run `./v1to2.sh`
* `cd _tmp/elevators`
* `./bin/elevators daemon_attach` to connect to the server
* have a play ...
* leave the console with `ctrl-D`
  * using `ctrl-C` or `ctrl-G` to exit will also stop the simulator!
* live switch between the two versions with:
  * `./bin/elevators downgrade 1.0`
  * `./bin/elevators upgrade 2.0`

_(End of update-1 notes)_

History
-------

Originally written by Håkan Huss in the late 90s at Erlang Systems.

It was part of the 5-day Erlang Literacy course aimed at test and
support staff of the GPRS project. It was used as a lab exercise
in several parts of the course to practise different topics such
as behaviours, supervisors, release handling, etc. The first exercise
using the elevator example was the mentioned "bug" that the elevator
always stopped at every floor (it is actually not a bug, but an
elevator scheduler which is trivially guaranteed to fulfill the
requirements on elevator scheduling used in the example). Several
refinements of the scheduling were made in the course lab exercises.

Since the purpose of the exercise was to use many different aspects
of Erlang and OTP, the code has been written to use many behaviours
rather than to be the "best" solution for the problem. The resulting
architecture should still be pretty good, though.

It has also been used to demonstrate how Quick Check can test
elevator systems, and featured in a few academic papers.

Thanks go to Francesco Cesarini and Håkan Huss for the historical
informations.

Quick start
-----------

 *  Make sure you have make and rebar installed and in your `$PATH`
 *  Type `make` to build everything
 *  Start the release with `./_rel/bin/elevators console`
 *  Make elevators move and notice they stop at each floor
 *  (Exercise: Find bugs, fix them and build a release upgrade)
 *  Place fixes in `upgrade/scheduler.erl`
 *  Run `./v1to2.sh`

Troubleshooting
---------------

The current compilation process prepares the demo entirely, but doesn't
double check things. If something fails during the compilation or the
release generation, you might want to reset the working directory.
