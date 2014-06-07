# Introduction

The RegistryApi allows easier access to registries, keys and values.  It can be very useful when processing complex keys or several registries in memory at once.  This wiki covers each of the functions contained in the RegistryApi.

# Basic Usage

In order to use the RegistryApi it must be imported and instantiated: 

    import volatility.plugins.registry.registryapi as registryapi
    ...
    
    def calculate(self):
        regapi = registryapi.RegistryApi(self._config)
    

or from `volshell`:

    >>> import volatility.plugins.registry.registryapi as registryapi
    >>> regapi = registryapi.RegistryApi(self._config)
    ...

At this point any of the RegistryApi functions may be used.

- **Functions**
	- [populate_offsets](Command-Reference-Registry-Api#populate_offsets)
	- [set_current](Command-Reference-Registry-Api#set_current)
	- [reset_current](Command-Reference-Registry-Api#reset_current)
	- [print_offsets](Command-Reference-Registry-Api#print_offsets)
	- [reg_get_currentcontrol](Command-Reference-Registry-Api#reg_get_currentcontrolset)
	- [reg_get_key](Command-Reference-Registry-Api#reg_get_key)
	- [reg_yield_key](Command-Reference-Registry-Api#reg_yield_key)
	- [reg_enum_key](Command-Reference-Registry-Api#reg_enum_key)
	- [reg_get_all_keys](Command-Reference-Registry-Api#reg_get_all_keys)
	- [reg_get_all_subkeys](Command-Reference-Registry-Api#reg_get_all_subkeys)
	- [reg_yield_values](Command-Reference-Registry-Api#reg_yield_values)
	- [reg_get_value](Command-Reference-Registry-Api#reg_get_value)
	- [reg_get_last_modified](Command-Reference-Registry-Api#reg_get_last_modified)