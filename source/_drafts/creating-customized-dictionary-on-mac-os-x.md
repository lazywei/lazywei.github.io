title: Creating a customized dictionary on Mac OS X
tags: ["Mac"]
---

Here is a note about how to create a customized dictionary on Mac OS X.

- Download the **Auxiliary Tools for Xcode** from https://developer.apple.com/downloads/index.action
- Move the Dictionary Development Kit folder to `/Applications/Utilities/DictionaryDevelopmentKit/` (without the spaces), and copy the `project_templates` folder to `~/Desktop/`
- Open `~/Desktop/project_templates/Makefile` and change `DICT_BUILD_TOOL_DIR` from `/DevTools/Utilities/Dictionary Development Kit` to `/Applications/Utilities/DictionaryDevelopmentKit`
- `cd ~/Desktop/project_templates/; make && make install`

ref: http://apple.stackexchange.com/questions/80099/how-can-i-create-a-dictionary-for-mac-os-x/86065#86065
