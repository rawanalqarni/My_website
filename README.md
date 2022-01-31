## Hi 
This repositery contain all the files for my website. I used a hugo theme called "starter-hugo-academic" to create my website. 

Steps to get started:
- if you are runing rstudio in rrcocker image, you have to download another image called **"pandoc/core"**
- In Rstduio install blogdown package >> **install.packages("blogdown")**
- Then create a new project go to **file >> _new project >> new directry >> choose "Website using Blogdown" option >> specify the directry name and the theme you want you can stay with the defult theme >> create project_**
  - **Note: The theme I used look like this [View](https://academic-demo.netlify.app/)**
- Serve your site by click addins button in the toolbar and choose serve site or you can run this command >> **blogdown:::serve_site()** 

That's it, **Congrats** now you can edit your website and have fun developing your own website. 

- One of the most important Command you need in this stage is creating new post: **blogdown::new_post(title="put a post title here")**
This will create a new folder under content/post that consist of a RMarkdown file where you can add you portfolios and project and show them in a nice way. 




