# kendo-webpack-error-2016.1.226+SP1
A test for kendo webpack problem introduced by the 2016 versions of kendo.  Please see http://www.telerik.com/forums/new-amd-definitions-in-v2016-1-112-are-incompatible-with-webpack for details

###The Bug
Webpack doesn't find dependant Kendo files.


###Environment Setup
The code in this repository uses Node/npm to install its dependencies.  Setup instructions are:

1. Install [Node/npm](https://nodejs.org/en/download/) if you don't have it already.  For Windows, you'll also need a bash shell, which you get if you install [Github for Windows] (https://desktop.github.com/) (make sure you tick the box to install the shell).
1. In a bash window, git clone this repository.
1. cd to the repository folder, then issue `npm install` to download the dependencies.
1. One of those dependencies will be Bower.  You can now use Bower to install Kendo Pro from the official Telerik Bower packages by issuing ` node_modules/.bin/bower install` at your command prompt.  You will be prompted to your Telerik ID and password at this stage, and may have to enter them twice.
1. Type `npm run start` to start webpack-dev-server.
1. Open the file src/index.html in your browser from this address. http://localhost:8080/src/index.html.  (The server in this case is webpack-dev-server.)

###Further Notes
My code uses Babel to transpile ES6 modules into ES5.  These extends to webpack itself, so my config file is actually called webpack.config.babel.js.

I've set up Webpack using the Telerik instructions at: http://docs.telerik.com/kendo-ui/third-party/webpack.  These instructions say to put the following entries in the config file:

    resolve: {
        extensions: [ '', '.js', 'min.js' ],
        root: [
            path.resolve('.'),
            path.resolve('../kendo/dist/js/') // the path to the minified scripts
        ]
    },

But it doesn't work for me.  Note: I did change the paths to point to kendo in the bower\_components folder, and to the source versions rather than the minified ones, like so:

    resolve: {
        extensions: [ '', '.js', 'min.js' ],
        root: [
            path.resolve('.'),
            path.resolve('../bower_components/kendo-ui/src/js/')
        ]
    },

Webpack cannot find the Kendo files.


###Workaround
I can work around the problem by hard-coding a path to my kendo-ui folder.  My resolve entry looks like so:

    resolve: {
        // Hard-coded path to kendo src files is only necessary due to this
        // bug: https://github.com/webpack/webpack/issues/1897
        // Note: this wasn't required in previous release of kendo
        modulesDirectories: ['node_modules', 'bower_components',
            path.join(__dirname, 'bower_components/kendo-ui/src/js')
        ]
    }

You'll find this setup in the webpack.config.babel.working.js file.





