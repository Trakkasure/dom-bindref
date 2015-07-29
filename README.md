# Polymer tag for template reference placement.

Simply put, it is like the "ref" attribute of the template tag from Polymer 0.5. It tries to place content from a template, possibly loaded in a different file, in the place of this tag.  It then attempts to bind properties from this and the parent template into the variable bindings in that referenced template.

## Example:
This is contrived and could be easily be implemented with another variable, but it's a dirt simple example.
I've also typed this in without testing.
```
    <template id="ref1">Howdy <span>{{arriving-person}}</span></template>

    <template id="ref2">Goodbye <span>{{departing-person}}</span></template>

    <template is="dom-bind" id="app">

        <paper-material elevation="2">
            <paper-button raised on-click="changeRef">Say Goodbye.</paper-button>
        </paper-material>

        <paper-material elevation="2">
            <dom-bindref id="myBind" ref="howdyTpl"></dom-bindref>
        </paper-material>
    </template>


    <script>
        var app = document.querySelector("#app");
        app.changeRef=function() {
            app.$.myBind.set("ref","ref2");
        };
        window.addEventListener('WebComponentsReady',function() {
            app.set('arriving-person',"new guy");
            app.set('departing-person',"old guy!");
        });

    </script>
```

## Not much better Example
This is a better example where you can create a simple list.
I've also typed all of this this in without testing.
```
    <link ref="import" href="./list-item-types.html">

    <template is="dom-bind"
    <iron-list items="[[data]]">
        <dom-bindref ref="[[item.type]]"></dom-bindref>
    </iron-list>

    <script>
        var app = document.querySelector("#app");
        app.showReports=function(e) {
            var model=e.model;
            // do more stuff.
        };
        window.addEventListener('WebComponentsReady',function() {
            // Populate the iron-list items array (app.data)
            app.set('data',[
                {id:1,type:"manager",fullName:"John Smith",org:"IT",reports:[2,3,4,5]},
                {id:2,type:"person",fullName:"Jane Doe"},
                {id:3,type:"person",fullName:"Bob Zane"},
                {id:4,type:"person",fullName:"Gillian Franklin"},
                {id:5,type:"person",fullName:"Paul Keebler"}
            ]);
        });
        
```
This is the list-item-types.html file. It could be included anywhere, but it must be a link somewhere.
```
    <template id="person">
        <paper-item-body two-line class="layout vertical regular-person">
            <div>
                <iron-icon icon$="[[iconForType(item.type)]]"></iron-icon>
                <div class="name">{{item.fullName}}</div>
            </div>
            <div secondary>A regular user</div>
        </paper-item-body>
    </template>

    <template id="manager">
        <paper-item-body two-line class="layout vertical manager">
            <div class="layout horizontal">
                <iron-icon icon$="[[iconForType(item.type)]]"></iron-icon>
                <div class="flex" class="name">{{item.fullName}}</div>
                <div class="control"><paper-button raised on-click="showReports">-&gt;</paper-button></div>
            </div>
            <div secondary>
                <div>Manager of <span>{{item.org}}</span></div>
                <div>with <span>{{items.reports.length}}</span direct reports.</div>
            </div>
        </paper-item-body>
    </template>
```


# TODO
I am in process of creating tests. So some things might be broken in browsers other than chrome.
