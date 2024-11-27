Python IDE on EOX Hub
For some users (like me), having a python IDE for execution & debugging as well as exploring allocating variable is essential. This applies mostly to scripting of python files (not notebooks). The Lab launcher offers the community edition of Visual Studio (VS Code icon) which allows to debug files. However, I don't see any allocated variables (when running a simple script with e.g. "a=7"). 

There is another icon "Python file" under the Launcher category "Other" but this only opens a new text windows. Executing python code works after "Creating a console for Editor"  but debugging does not appear to be possible there (or exploring variable content).

![image](https://github.com/user-attachments/assets/4e6623d7-0a21-4dea-b587-9167c00e6868)

I can add the following. when running a python script (*.py) in VS code, one can either [in theory] see variable content through the debugging run of a script 

![grafik](https://github.com/user-attachments/assets/23eb61b1-69f6-4288-89fa-ef59b5df5c68)

or by executing the script in the interactive window (right click within script window)

![grafik](https://github.com/user-attachments/assets/c3709378-69fd-490f-80db-ed0296d7c01e).

in both cases I should see the content of allocated variables. the interactive window is unpractial as it has to be restarted from the script every time, i.e. the interactive window  does not refresh with changes in the script. the debugging option is slower than standard execution but should allow simple rerun of changes of the script. however under the EOX Lab VS code application, only the interactive window gives you the variables (center bottomg panel of the image below), the debugging variable window (left panel in image below) is empty (but it works on local system)

![grafik](https://github.com/user-attachments/assets/26fcc40f-9d5e-4311-8128-bfa2b5bf5e3c)