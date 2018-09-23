README-WWW2019

SEPTEMBER 2019

academic-2019-flipucr: personal website, to be put into theory.ucr subdir
academic-2019-ucrhep: redo of UCR websie

Hugo Theme Academic Source:
https://sourcethemes.com/academic/docs/install/

DIRECTORY STRUCTURE

This is a little tricky. The idea is that there are hugo folders ("academic-...") that contain the source files for generating the static site. These sites output to www2019, which is what should be synchronized to GitHub/Netlify.

Here's the subtle part: personal websites are a subset of the group website. So want the group website to output to www2019/ while personal websites should output to www2019/~flip/.

This means one has to set the `publishDir` config.toml variable carefully in each website. 

By doing this, the group website and the pesonal websites are independent static websites.