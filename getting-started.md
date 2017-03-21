# Getting Started

## Requirements

* Install the [package DocuPanel](https://www.nuget.org/packages/DocuPanel/).
* Modify the platform in your configuration manager to be x64.
* A documentation written in Markdown.

DocuPanel uses the Chromium Browser [CefSharp](https://cefsharp.github.io/), that is why your application has to be x64 and not Any CPU or x86. However you can also download the [source code](https://github.com/RHEAGROUP/docupanel) and build it in x86.

## Integration

### In your code

DocuPanel is a UserControl that needs to be integrated inside a window or another ContentControl. To integrate it you need to add to your view the following code block

```xaml
<docuPanel:DocumentationView
            PathDocumentationIndex="C:\Projects\DocuPanel\DocuPanelSupport\bin\x64\Debug\Documentation\book.json"
            RootAppData="Users\<userName>\AppData\Local\<YourAppDataFolder>"
            UpdateIndexation="true"/>
```

**PathDocumentationIndex** `string` which corresponds to the path of the index file of your documentation. This file must be a `.json` file and be present at the root of the documentation.

**RootAppData** `string` which corresponds to the path of the application data folder of your application.  
DocuPanel will create on this path a directory called `DocuPanel` to store its datas.  

**UpdateIndexation** `bool` which indicates whether the indexation needs to be updated.  
If true, DocuPanel will browse all the files indicated in the index, and will convert them into HTML if they don't already exist. DocuPanel converts your Markdown files into HTML files to be displayed by CefSharp. The indexation for the searches will also be updated with the new documentation content. Note that if you want to update the content of a file, to consider the changes you have to delete the HTML file from the application data folder. This property needs to be `true` the first time you use DocuPanel and each time you make changes in your documentation.

### Structure of your documentation ###
Your documentation files should be present inside your project directory and can be ordered the way you want. However they must have different names.

`Pages` should have the following properties  

| Build Action | Copy to Outpout Directory |
|:-------------|:--------------------------|
| None         | Copy if newer             |

The `index` must be `.json` file and should have the same properties.

It has to be structured as the following example
```json
{  
  "Title": "Documentation",
  "Author": "RHEA System S.A.",
  "PagePath": "index.md",
  "Sections": [
    {
      "Name": "Quick Start",
      "PagePath": "quickstart_Quick_Start.md"
    },
    {
      "Name": "Installation",
      "Sections": [
        {
          "Name": "Introduction",
          "PagePath": "Installation\\1_installintro_Introduction.md"
        },
        {
          "Name": "Configuration",
          "Sections": [
            {
              "Name": "Step by Step",
              "PagePath": "Installation\\2_installstep_Step_by_Step.md"
            },
          ]
        }
      ]
    }
  ]
}
```

The structure of the previous code gives

![](/assets/hierarchy.PNG "Image")

**Title **is the title of you documentation. It will appear on the left top corner of the DocuPanel.

**Author **is the author of the documentation. Can be empty. It is actually not displayed.

**PagePath **is the path of the page. Note that a section does not necessary contains PagePath. For example a section can contain only children pages, it's what happens with\_Installation\_and\_Configuration\_in our example. You can add a main page to your documentation which will appear when a user will click on the top left corner where is written the title of your documentation. To do that you have to write the path of this page in the PagePath located just after the Author.

**Sections **is the list of the subsections. The treeView displayed on the left contains only sections, which are entities of your doc. Sections can be associated to a page or just contains other sections.

**Name **is the name displayed for a section in the treeView. It is possible to have two sections with the same name.

## Sample

We developed a simple example to show you how it works. The example is a project which only contains a window which includes DocuPanel. The [Documentation](https://github.com/RHEAGROUP/docupanel/tree/master/DocuPanelSupport/Documentation) folder contains the files that DocuPanel needs to consider. book.json is our index file which contains all the information relative to the documentation. Take a look at the [example](https://github.com/RHEAGROUP/docupanel/tree/master/DocuPanelSupport).

