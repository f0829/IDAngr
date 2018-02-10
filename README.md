# IDAngr

<p align="center">
<img src="http://andreafioraldi.altervista.org/idangr.png">
</p>

Use [angr](https://github.com/angr/angr) in the IDA Pro debugger generating a state from the current debug session.

> Very naive version, it works but i must improve it
>
> The GUI is not yet usable
>
> If you want to contribute pr are accepted

## Usage

1. Load the idangr.py script from the ida menu to create an angr project
2. During the debug create an instance of StateManager or StateShot
3. Do things with angr
4. Return to 2 or exit

## Api

#### StateShot

Return an angr state from the current debug session state.

#### StateManager

A wrapper around angr to simplify the symbolic values creation and to write the results back in the debugger when angr founds a valid path.

##### Methods
+ `instance.sim(key, size)`        create a symbolic value on a register or on a memory address (size is optional)
+ `instance[key]`                  get a register or a memory value
+ `instance.simulation_manager()`  create an angr simulation manager based on the state
+ `instance.to_dbg(found_state)`   transfer to the debugger state the evaluated value of the symbolic value created before with sim

note: memory values are the same that are returned by `state.mem[addr]`

## Example

```python
Python>sm = StateManager()
Python>sm.sim("edi")
Python>sm.sim("esi")
Python>m = sm.simulation_manager()
Python>m.explore(avoid=0x04005D5, find=0x00004005BC)
<SimulationManager with 1 found, 3 avoid>
Python>idc.GetRegValue("edi")
0
Python>idc.GetRegValue("esi")
5
Python>sm.to_dbg(m.found[0])
Python>idc.GetRegValue("edi")
2
Python>idc.GetRegValue("esi")
0
```

See [examples](https://github.com/andreafioraldi/IDAngr/tree/master/examples) folder.

## TODO
+ improve memory read from debugger
+ create abstract classes to work with different debuggers
+ gui for IDA (idangr_gui.py)


