# TREC Interactive Knowledge Assistance Track
Website for the TREC iKAT track. Made with [Mkdocs](https://www.mkdocs.org/) using [Material](https://squidfunk.github.io/mkdocs-material/) theme.  

## Requirements
You need to have `mkdocs` and `mkdocs-material` installed on your system. You can use `pip` for this. See the official documentation of `mkdocs` and `mkdocs-material` for detailed installation instructions. 

## Making changes to the website
See the official documentation of Mkdocs and Material for details on what changes can be made and how to make them. Below, I summarize a few important points. 
- To change the color, font, etc. of the website, you need to make changes to the `mkdocs.yml`file. See the official Material webiste for details. 
- To add a new page, add a `.md` file to the `docs` folder. Name the file appropriately. 
- This website uses Github Pages for deployment. You can see local changes made on your system using `mkdocs serve`. This will open the website on `http://127.0.0.1:8000` on your system. 
- Once you are happy with the changes, you can deploy the website using `mkdocs gh-deploy`. 
