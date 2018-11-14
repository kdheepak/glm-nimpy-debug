# glm-nimpy-debug


### Summary

- Install nimpy and switch to pr 65
- Install glm
- Test Python module with file in this repo

### Instructions

- Install glm

```
git clone https://github.com/NREL/glm
cd glm
nimble install
```

- Ensure nimpy is being used from this branch: https://github.com/yglukhov/nimpy/pull/65

```
cd ..
git clone https://github.com/kdheepak/nimpy
git checkout kd/add-python-exception-from-nim
nimble install -y
cd ../glm
```

- Test `glm2json` command line version

```
nim c -d:release -r src/glm2json.nim --pathtofile ./IEEE_13_Node_Test_Feeder.glm # check that this works properly and dumps json to screen
```

- Test `glm` python module. This should segfault to replicate the error.

```

touch t.py

cat >t.py <<EOL
echo import sys

sys.path.append("./lib")

import glm
import json

json.loads(glm.load("./IEEE_13_Node_Test_Feeder.glm"))
EOL

nim c --passC:-g -d:release --app:lib --out:lib/glm.so src/glm.nim; python t.py
Hint: used config file '/Users/$USER/.choosenim/toolchains/nim-0.19.0/config/nim.cfg' [Conf]
Hint: used config file '/Users/$USER/GitRepos/glm/config.nims' [Conf]
Not running windows
Hint: system [Processing]
Hint: glm [Processing]
Hint: utils [Processing]
Hint: logging [Processing]
Hint: strutils [Processing]
Hint: parseutils [Processing]
Hint: math [Processing]
Hint: bitops [Processing]
Hint: algorithm [Processing]
Hint: unicode [Processing]
Hint: times [Processing]
Hint: options [Processing]
Hint: typetraits [Processing]
Hint: strformat [Processing]
Hint: macros [Processing]
Hint: posix [Processing]
Hint: os [Processing]
Hint: ospaths [Processing]
utils.nim(3, 6) Hint: 'utils.initLogger()[declared in utils.nim(3, 5)]' is declared but not used [XDeclaredButNotUsed]
Hint: json [Processing]
Hint: hashes [Processing]
Hint: tables [Processing]
Hint: lexbase [Processing]
Hint: streams [Processing]
Hint: parsejson [Processing]
Hint: nimpy [Processing]
Hint: dynlib [Processing]
Hint: complex [Processing]
Hint: sequtils [Processing]
Hint: py_types [Processing]
Hint: py_utils [Processing]
Hint: strscans [Processing]
Hint: py_lib [Processing]
Hint: parser [Processing]
Hint: lexer [Processing]
Hint: tokens [Processing]
lexer.nim(88, 6) Hint: 'lexer.match(lex: var Lexer, expected: char)[declared in lexer.nim(88, 5)]' is declared but not used [XDeclaredButNotUsed]
Hint: ast [Processing]
glm.nim(20, 6) Hint: 'glm.parse(file: string)[declared in glm.nim(20, 5)]' is declared but not used [XDeclaredButNotUsed]
CC: nim_glm_parser
CC: nim_glm_lexer
Hint:  [Link]
Hint: operation successful (64043 lines compiled; 1.011 sec total; 74.672MiB peakmem; Release Build) [SuccessX]
SIGSEGV: Illegal storage access. (Attempt to read from nil?)
```

