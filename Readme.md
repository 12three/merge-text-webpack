Merge Files Webpack Plugin
==========================

This is a small plugin created to merge files extracted by 
the **extract-text-webpack-plugin** into a single one.
It is useful, for example, when you have multiple entries 
in your webpack configuration and you use the **extract-text-webpack-plugin** 
plugin to extract all the **.css** files into a separate one. Without this plugin,
**extract-text-webpack-plugin** will extract a **.css** file per entry (if you use 
have set the filename as *[name].css*) or in the worst case a single file with only 
the **.css** file of the last entry (if you have set the filename as *style.css*).

With this plugin, you can extract all the **.css** files from all entries 
into a single **.css** file.

# Use

Install:

    npm install merge-text-webpack-plugin --save-dev

You need to use **webpack 2.\*** and **extract-text-webpack-plugin 2.0.0\***.
The filename for the **extract-text-webpack-plugin** has to use **[name].something.ext** 
where *something.ext* isthe name of the final file that you want to create. For this 
plugin, use then *something.ext* as the filename in the options objects.

A sample file would be:

    var webpack = require('webpack');
    var path = require('path');
    var ExtractTextPlugin = require('extract-text-webpack-plugin');
    var MergeFilesPlugin = require('merge-files-webpack-plugin');

    module.exports = {
        entry: {
            'entry1': './tests/test1/src/entry1/index.js',
            'entry2': './tests/test1/src/entry2/index.js'
        },
        output: {
            path: path.join(__dirname, './tests/test1/public'),
            filename: '[name].js'
        },
        module: {
            rules: [
                {
                    use: ExtractTextPlugin.extract({
                        use: 'css-loader'
                    }),
                    test: /\.css$/,
                    exclude: /node_modules/
                }
            ]
        },
        plugins: [
            new ExtractTextPlugin({
                filename: '[name].style.css'
            }),
            new MergeFilesPlugin({
                filename: 'style.css'
            })
        ]
    }

Check the test directory in https://github.com/jtefera/merge-files-webpack/tests/test1