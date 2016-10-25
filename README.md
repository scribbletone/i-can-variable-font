# I Can Variable Font (Notes on generating variable fonts)

Notes and tips for generating a simple variable font on a Mac. These assume you’re already familiar with the UFO format, creating masters that can be interpolated, and have some comfort using Terminal.

There are probably better ways of doing this. If anyone has better sources, corrections, or the process changes, please send a pull request or [open an issue](https://github.com/scribbletone/i-can-variable-font/issues). Hopefully this is just an interim document until the process becomes streamlined.

## Example Files
In the `example` directory, you can find a sample DesignSpace file and interpolatable UFO’s. You can use those files for your first attempt, to simplify the process, and make sure everything works. It only contains one glyph, an `A`, which is just a rectangle that should get taller and shorter.

## How To 
1. Install `pip`
  - Check to see if you have it installed already.
    - In Terminal type `pip` and hit enter. 
      - If it says `command not found`, follow the [instructions here](https://pip.pypa.io/en/latest/installing/#install-or-upgrade-pip). Otherwise skip to step 2.
2. Install `fontmake`
  - First [clone or download the repository](https://github.com/googlei18n/fontmake). 
    - You can save this anywhere you like, but you’ll need it every time you generate the fonts. So put it somewhere that makes sense for your file organization.
    - If you download it as zip, remove '-master' from the directory’s name.
  - In Terminal, [navigate](https://github.com/scribbletone/i-can-variable-font#terminal-tips) to the new `fontmake` directory you just downloaded.
  - Follow the instructions in their [readme](https://github.com/googlei18n/fontmake).
3. Create a DesignSpace file
  - create a new text file called `yourfont.designspace`
  - Populate the file using the following examples as a guide. Most importantly, make sure the paths to the UFOs are correct. https://github.com/scribbletone/i-can-variable-font/blob/master/example/varibox.designspace and https://github.com/LettError/MutatorMath/blob/master/Docs/designSpaceFileFormat.md
  - Add at least one instance
4. Generate interpolatable TTFs
  - In Terminal, [navigate](https://github.com/scribbletone/i-can-variable-font#terminal-tips) to the `fontmake` directory.
    - If you’ve closed the Terminal window since installing, you’ll also need to run `source env/bin/activate`.
  - run `fontmake -o ttf-interpolatable -m path-to-your-designspace-file`. 
    - Make sure to substitute your path to the DesignSpace file.
  - If all goes well, you should now have TTFs in the `fontmake/master_ttf_interpolatable` directory.
5. Generate the final variable font
  - Copy the generated TTFs from the previous step, and place them in the same directory as your source UFOs.
  - Make sure the TTFs have the same file name as your UFOs(without the file extension). If not, you’ll get an error. 
  - From the `fontmake` directory run `python env/lib/python2.7/site-packages/fontTools/varLib/__init__.py path-to-your-designspace-file`. 
    - Again, make sure to substitute your path to the DesignSpace file.
  - Cross your fingers :)
  - If everything goes well, you should end up with a new TTF file next to the DesignSpace with `-GX` in the name.

## Weird Things
- Your sources seem to need a `GPOS` or kerning table. In the example file, I got around that by just creating a single kerning pair with a value of 0.
- If you don’t have groups in your UFO, don’t include `<groups copy="1"/>` in your DesignSpace file. It’ll throw an error.
- In RoboFont, using relative paths when including external feature files can throw a `No such file or directory` error, as [seen here](https://github.com/googlei18n/fontmake/issues/157)
  - To fix, change your relative paths from something like: `include(../features.fea);` to `include(../../features.fea);`
- Superpolator instances that are based on extrapolations are clamped in the generated v-fonts. For example, if your boldest master has a weight value of 800, an instance with an extrapolated weight value of 1000 will appear the same as 800 in the generated font.
- Superpolator substitution rules (http://new.superpolator.com/documentation/rules/) get lost somewhere between the designspace file and the generated v-font. Or perhaps the generated fonts are legit and the apps that currently support v-fonts just don't support that feature yet?
- Apps that currently support v-fonts only seem to support one family name per font (Still could be tested more thoroughly.)
- If you’re utilizing any custom axes that aren’t among the handful of [registered](https://www.microsoft.com/typography/otspec180/fvar.htm#VAT) axis tags, you need to write them into the `standard_axis_map` dictionary in the `/fontTools/varLib/__init__.py` file.

## ‘Using’ the fonts
- Mac previewer https://github.com/googlei18n/fontview/releases
- Webkit nightly https://webkit.org/downloads/

## Helpful Resources and Articles
- [Fontmake](https://github.com/googlei18n/fontmake) by Google
- [FontTools](https://github.com/fonttools) a community project with contributions by [@justvanrossum](https://github.com/justvanrossum), [@behdad](https://github.com/behdad), [@anthrotype](https://github.com/anthrotype), [@brawer](https://github.com/brawer), and many more. 
  - In particular [varLib](https://github.com/fonttools/fonttools/blob/master/Lib/fontTools/varLib/__init__.py#L13-L17)
- [MutatorMath](https://github.com/LettError/MutatorMath) by [LettError](http://letterror.com/)
- [OpenType Font Variations Overview](https://www.microsoft.com/typography/otspec180/otvaroverview.htm) by Microsoft
- [Introducing OpenType Variable Fonts](https://medium.com/@tiro/https-medium-com-tiro-introducing-opentype-variable-fonts-12ba6cd2369#.imv0hzmro) by [John Hudson](http://www.tiro.com/)
- [Variable Fonts](http://typographica.org/on-typography/variable-fonts/) by [CJ Dunn](http://thecjdunn.com/)

## Examples/Demos
- http://cjtype.com/dunbar/variablefonts/index.html
- http://stuff.djr.com/variable-demo/

## Terminal Tips
- To quickly get the path to a directory/folder or file, drag it into terminal.
- To navigate to a directory in Terminal, type `cd ` then drag the folder into terminal and hit enter.
- To see the contents of the current directory enter `ls`. To see hidden files as well, enter `ls -a`.
- To navigate relative to your current location `cd some_folder/another_folder`
- To cycle through recently used commands, hit the up arrow key.
- To clear the window of noise from previous output, hit the `command k` keys.

## Outside Contributors
Thanks to [@nicksherman](https://github.com/nicksherman) for feedback and revisions, and [@cjdunn](https://github.com/cjdunn) for pointing the way.

Help us make this guide better! Open a pull request or [issue](https://github.com/scribbletone/i-can-variable-font/issues) with any suggestions or corrections.
