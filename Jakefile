/*
 * Jakefile
 * foo
 *
 * Created by You on June 25, 2010.
 * Copyright 2010, Your Company All rights reserved.
 */

var ENV = require("system").env,
    FILE = require("file"),
    JAKE = require("jake"),
    task = JAKE.task,
    FileList = JAKE.FileList,
    app = require("cappuccino/jake").app,
    configuration = ENV["CONFIG"] || ENV["CONFIGURATION"] || ENV["c"] || "Debug",
    OS = require("os");

app ("LPKit-Examples", function(task)
{
    task.setBuildIntermediatesPath(FILE.join("Build", "foo.build", configuration));
    task.setBuildPath(FILE.join("Build", configuration));

    task.setProductName("LPKit-Examples");
    task.setIdentifier("com.yourcompany.foo");
    task.setVersion("1.0");
    task.setAuthor("Your Company");
    task.setEmail("feedback @nospam@ yourcompany.com");
    task.setSummary("LPKit-Examples");
    task.setSources((new FileList("**/*.j")).exclude(FILE.join("Build", "**")));
    task.setResources(new FileList("Resources/**"));
    task.setIndexFilePath("index.html");
    task.setInfoPlistPath("Info.plist");

    if (configuration === "Debug")
        task.setCompilerFlags("-DDEBUG -g");
    else
        task.setCompilerFlags("-O");
});

task ("LPAristo", function()
{
    JAKE.subjake(["LPAristo"], "build", ENV);
});

task ("default", ["LPAristo", "LPKit-Examples"], function()
{
    printResults(configuration);
});

task ("build", ["default"]);

task ("debug", function()
{
    ENV["CONFIGURATION"] = "Debug";
    JAKE.subjake(["."], "build", ENV);
});

task ("release", function()
{
    ENV["CONFIGURATION"] = "Release";
    JAKE.subjake(["."], "build", ENV);
});

task ("run", ["debug"], function()
{
    OS.system(["open", FILE.join("Build", "Debug", "LPKit-Examples", "index.html")]);
});

task ("run-release", ["release"], function()
{
    OS.system(["open", FILE.join("Build", "Release", "LPKit-Examples", "index.html")]);
});

task ("deploy", ["release"], function()
{
    FILE.mkdirs(FILE.join("Build", "Deployment", "LPKit-Examples"));
    OS.system(["press", "-f", FILE.join("Build", "Release", "LPKit-Examples"), FILE.join("Build", "Deployment", "LPKit-Examples")]);
    printResults("Deployment")
});

task ("desktop", ["release"], function()
{
    FILE.mkdirs(FILE.join("Build", "Desktop", "LPKit-Examples"));
    require("cappuccino/nativehost").buildNativeHost(FILE.join("Build", "Release", "LPKit-Examples"), FILE.join("Build", "Desktop", "LPKit-Examples", "foo.app"));
    printResults("Desktop")
});

task ("run-desktop", ["desktop"], function()
{
    OS.system([FILE.join("Build", "Desktop", "LPKit-Examples", "foo.app", "Contents", "MacOS", "NativeHost"), "-i"]);
});

function printResults(configuration)
{
    print("----------------------------");
    print(configuration+" app built at path: "+FILE.join("Build", configuration, "LPKit-Examples"));
    print("----------------------------");
}
