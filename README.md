# I Can Variable Font (Notes on generating variable fonts)

Notes and tips for generating a simple variable font on a Mac. These assume you’re already familiar with the UFO format, creating masters that can be interpolated, and some comfort using Terminal.

There are probably better ways of doing this. If anyone has better sources, corrections, or the process changes, please send a pull request or contact us. Hopefully this is just an interim document until the process becomes streamlined.

## Example Files
In the `example` directory, you can find a sample DesignSpace file and interpolatable UFO’s. You can use those files for your first attempt, to simplify the process, and make sure everything works.

## The Magic 
1. Install `fontmake`
  - First [clone or download the repository](https://github.com/scribbletone/fontmake).
    - Note the official repository is [actually here](https://github.com/googlei18n/fontmake), but there’s currently a bug. I’ll switch the link back as soon as the bug is addressed in the official repository.
  - In Terminal, navigate to the new `fontmake` repository you just downloaded.
  - Follow the instructions in their [readme](https://github.com/googlei18n/fontmake).
2. Create a DesignSpace file
  - create a new text file called `yourfont.designspace`
  - Link up your master UFOs using the following examples as a guide https://github.com/scribbletone/i-can-variable-font/blob/master/example/varibox.designspace and https://github.com/LettError/MutatorMath/blob/master/Docs/designSpaceFileFormat.md
  - Add at least one instance
3. Generate interpolatable TTFs
  - In Terminal, navigate to the `fontmake` directory.
    - If you’ve closed the Terminal window since installing, you’ll need to run `source env/bin/activate`.
  - run `fontmake -o rtf-interpolatable -m path-to-your-designspace-file`. 
    - Make sure to substitute your path to the DesignSpace file.
  - If all goes well, you should now have TTFs in the `fontmake/master_ttf_interpolatable` directory.
4. Generate the final variable font
  - Copy the generated TTFs from the previous step, and place them in the same directory as your source UFOs.
  - Make sure the TTFs have the same file name as your UFOs. If not, you’ll get an error. 
  - From the `fontmake` directory run `python env/lib/python2.7/site-packages/fontTools/varLib/__init__.py path-to-your-designspace-file`. 
    - Again, make sure to substitute your path to the DesignSpace file.
  - Cross your fingers :)
  - If everything goes well, you should end up with a new TTF file next to the DesignSpace with `-GX` in the name.

## Weird Things
- Your sources seem to need a `GPOS` or kerning table. In the example file, I got around that by just creating a single kerning pair with a value of 0.
- If you don’t have groups in your UFO, don’t include `<groups copy="1"/>` in your DesignSpace file.

## ‘Using’ the fonts
- Mac previewer(requires running a script to build the application) https://github.com/googlei18n/fontview
- Webkit nightly https://webkit.org/downloads/

## Helpful Resources and Articles
- [Fontmake](https://github.com/googlei18n/fontmake) by Google
- [Fonttools](https://github.com/fonttools) and in particular [varLib](https://github.com/fonttools/fonttools/blob/master/Lib/fontTools/varLib/__init__.py#L13-L17) by Just van Rossum, [LettError](http://letterror.com/)
- [MutatorMath](https://github.com/LettError/MutatorMath) by [LettError](http://letterror.com/)
- [OpenType Font Variations Overview](https://www.microsoft.com/typography/otspec180/otvaroverview.htm) by Microsoft
- [Introducing OpenType Variable Fonts](https://medium.com/@tiro/https-medium-com-tiro-introducing-opentype-variable-fonts-12ba6cd2369#.imv0hzmro) by [John Hudson](http://www.tiro.com/)
- [Variable Fonts](http://typographica.org/on-typography/variable-fonts/) by [CJ Dunn](http://thecjdunn.com/)

## Examples/Demos
- http://cjtype.com/dunbar/variablefonts/index.html
- http://stuff.djr.com/variable-demo/
