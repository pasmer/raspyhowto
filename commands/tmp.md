# TMP

To be reviewd



```

install.packages("remotes")
remotes::install_github("rspatial/terra")
remotes::install_github("r-spatial/mapview")
remotes::install_github("ropensci/rnaturalearthhires")


remotes::install_github("r-spatial/sf")
remotes::install_github("r-spatial/leafpop")

remotes::install_github("r-spatial/sf", configure.args = "--with-proj-lib=/usr/lib/grass76/etc/proj")
/usr/lib/grass76/etc/proj

pkg-config --libs proj

# list all packages where an update is available
old.packages()

# update all available packages
update.packages()

# update, without prompts for permission/clarification
update.packages(ask = FALSE)



rmarkdown::render('file.Rmd', output_file='file.html')


rmarkdown::render('file.Rmd', output_file='file.pdf')

```
