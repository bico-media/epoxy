Status: __idea__ → _rough draft_ → _draft_ → _proposal_ → _final review_ → _stable_


# Epoxy - Push all the data



> Let's get a common way of placing data _within_ the executed part of the script to ensure larger data size with default mining settings.

This document describes a protocol named "Epoxy" that will let what is currently known as OP_RETURN data be part of the executed script to a) ensure larger amounts of data to be added into one transaction with default mining configuration and b) let people have a way of using the data as part of the output puzzle for the transaction. 
Please share [inputs and comments](https://github.com/bico-media/epoxy/issues). 



## Background

### Current statue of OP_RETURN

WIP:

- A de-facto standard for adding data
- Predictability of the placement of data is key to the usability in the ecosystem. 


### The problem

- Default mining configuration has different limits for data that is part of the executed code and data that is after the executed code (`OP_RETURN` data)

- Data before `OP_DROP` or after `OP_RETURN` can relatively easily be pruned by cutting off the rest of the data. (see [this](https://blog.bsv.sh/the-prunability-and-non-permanency-of-your-data-the-case-of-op_return/) for details)




# Epoxy protocol definition

Epoxy should be considered a wrapper of current data - so it's about how data is stored and now what data is stored. 

At this stage, there are two suggestions to solve this awaiting feedback from the community. 

## Suggestion A

All of the script is valid. 

The first 3 instructions _must_ be `OP_NOP OP_TRUE OP_NOTIF`

This sequence has been selected based on it being highly unlikely to be used because it does not make sense to sure regarding calculations, meaning the.



```
OP_NOP OP_TRUE OP_NOTIF
[pushdata size element1]
[pushdata size element2]
[pushdata size element3]
OP_ELSE
[Optional script]
OP_ENDIF
```

The optional script needs to be valid for the transaction to be accepted.

This indicates that the relevant data is placed in the `n*3` location until `n*3+1` = OP_ELSE

A challenge with this notation is that data can be placed in different ways (smaller than 75 can be added with just a single instruction) so it's suggested to always use OP_PUSHDATA1, OP_PUSHDATA2 or OP_PUSHDATA4


## Suggestions B

Parts of the script is invalid. It will be the smallest change for the ecosystem but at the moment it's unclear what is the effects of having invalid code as part of the script. 

The first 3 instructions _must_ be `OP_TRUE OP_NOTIF OP_RETURN`


```
OP_TRUE OP_NOTIF OP_RETURN
[data just like opreturn today]
OP_ELSE
[whatever script]
OP_ENDIF
```

This indicates that the relevant data is placed in the `n+3` location until `n+3+1` = OP_ELSE



----

Great inspiration: https://blog.bsv.sh/the-prunability-and-non-permanency-of-your-data-the-case-of-op_return/




----

Please share [inputs and comments](https://github.com/bico-media/epoxy/issues).