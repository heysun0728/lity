VM Opcodes
==========


Overview
--------
Lity's EVM is a stack-based, big-endian VM with a word size of 256-bits and is used to run the smart contracts on the blockchain.
Each opcode is encoded as one byte.


Arithmetic
----------
* integer : UADD, UMUL, USUB, SADD, SSUB, SMUL
* fixed-point : UFMUL, SFMUL, UFDIV, SFDIV

The values are popped from the operand stack, 
and the result will be pushed back to the stack.

All arithmetic operations are overflow-checked.
An overflow would cause the contract execution fail. 
See :doc:`overflow protection <overflow-protection>` for more details.


ISVALIDATOR
-----------
This opcode can be used to check whether an address popped from the stack is a validator of CyberMiles blockchain.
With this feature, smart contract developers can create trusted smart contracts that can only be modified by the validators.
See :doc:`validator only contract <validator-only-contract>` for more details.


FREEGAS
-------
This operation allows developers to use contract balance to pay the transaction fee for the users.
See :doc:`freegas <freegas>` for more details.


RAND
----
This opcode can be used to generate a random number locally.
See :doc:`rand <rand>` for more details.


Opcodes
-------

+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| OpCode        | uint8        | Stack Input| Stack Output       | Expression                | Notes                                                  |
+===============+==============+============+====================+===========================+========================================================+
| `UADD`        | `0x01`       | `[a|b|$`   | `[a+b|$`           | `a + b`                   | unsigned integer addition                              |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `UMUL`        | `0x02`       | `[a|b|$`   | `[a*b|$`           | `a * b`                   | unsigned integer multiplication                        |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `USUB`        | `0x03`       | `[a|b|$`   | `[a-b|$`           | `a - b`                   | unsigned integer substraction                          |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `SADD`        | `0x0c`       | `[a|b|$`   | `[a+b|$`           | `a + b`                   | signed integer addition                                |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `SSUB`        | `0x0d`       | `[a|b|$`   | `[a-b|$`           | `a - b`                   | signed integer substraction                            |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `SMUL`        | `0x0e`       | `[a|b|$`   | `[a*b|$`           | `a * b`                   | signed intger multiplication                           |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `UFMUL`       | `0x2a`       | `[a|b|N|$` | `[a*b/pow(10,N)|$` | `A * B` [1]_              | unsigned fixed-point multiplication                    |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `SFMUL`       | `0x2b`       | `[a|b|N|$` | `[a*b/pow(10,N)|$` | `A * B` [1]_              | signed fixed-point multiplication                      |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `UFDIV`       | `0x2c`       | `[a|b|N|$` | `[a*pow(10,N)/b|$` | `A / B` [1]_              | unsigned fixed-point division                          |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `SFDIV`       | `0x2d`       | `[a|b|N|$` | `[a*pow(10,N)/b|$` | `A / B` [1]_              | signed fixed-point division                            |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `ENI`         | `0xf5`       |            |                    |                           |                                                        |  
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `ISVALIDATOR` | `0xf6`       | `[addr|$`  | `[boolean|$`       | `isValidator(addr)`       | make a contract can only be modified by the validators |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `SCHEDULE`    | `0xf7`       |            |                    |                           |                                                        |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `FREEGAS`     | `0xf8`       | `[|$`      | `[|$`              | function modifier freegas | let contract owner pay the tx fee for you              |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+
| `RAND`        | `0xf9`       | `[|$`      | `[keccak256|$`     | `rand()`                  | get the random number on the chain                     |
+---------------+--------------+------------+--------------------+---------------------------+--------------------------------------------------------+

.. [1] A = a * pow(10,N), B = b * pow(10,N)