#### How to add more docs
1.) Name your file using the following pattern: `#-name-of-doc.md`
2.) Place `#-name-of-doc.md` in `guides/docs`
3.) Create an `.ejs` file named `name-of-doc.ejs` at the root of `guides`
4.) Embed the `guide` partial within `name-of-doc.ejs` and pass in a configuration object which matches `guides/docs`. For example:
```
<%- partial('_partials/guide', {
    title: 'Page Title',
    selectedGuide: '../docs/1-cli-setup'
}) %>
```
