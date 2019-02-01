---
title: "Unity - Custom Editor 101"
date: 2019-02-01T18:50:52+07:00
draft: false
tags: ["unity", "custom-editor"]
---

## Introduction
Even though, Unity has a lot of standard tools. There is a time when we want to speed up our development.
We need the tools that understand our game to make ourself more comfortable. That's why we want to extend our Unity Editor.
Introduces "Custom Editor"

## What about "ExecuteInEditMode"?
**ExecuteInEditMode** attribute is the easiests way to make script run in Editor
~~~
using UnityEngine;

[ExecuteInEditMode]
public class Log : MonoBehaviour
{
    void Update()
    {
        Debug.Log("Editor cause this update.");
    }
}
~~~  
But we can't do much. It can't modify the way UI is draw in the Editor.  
It suits for a simple script that we want to see its result during the Editor, Not suit for a tools that we will use in the Editor.

## Hello World (Custom Menu)
**Script** must be in the folder name **"Editor"**  
If there is no exists folder, you have to create one. I'll create that folder in "Assets/Editor".
Then, We'll try to create a Custom Menu.
When we select this menu, It'll show a simple dialog.  
~~~
//Greet.cs
using UnityEngine;
using UnityEditor;

public class Greet : MonoBehaviour
{
    [MenuItem("Custom/Greet")]
    static void ShowGreetDialog()
    {
        EditorUtility.DisplayDialog("Greet", "Hello World~", "Okay", "");
    }
}
~~~
If we go to the Menu "Custom > Greet", Dialog will greet to us. hooray~ xD

## Let's make something useful
Every game project, There are atleast the folders "Script, Scenes, Prefabs, etc.."  
Let's make a script to auto create these folders.  
~~~
//FolderTemplate.cs
using UnityEngine;
using UnityEditor;

public class FolderTemplate : MonoBehaviour
{
    const string PARENT_FOLDER = "Assets";
    static readonly string[] Folders = {
        "Sprites",
        "Scenes",
        "Scripts",
        "Sound",
        "Prefabs"
    };

    [MenuItem("Custom/FolderTemplate/Create new template folders %#F12")]
    static void CreateFolderTemplate()
    {
        bool isConfirmCreate = EditorUtility.DisplayDialog(
                        "Warning",
                        "Are you sure to create a folders?",
                        "Yes",
                        "No");

        if (!isConfirmCreate)
            return;

        foreach (string path in Folders) {
            bool isFolderExist = AssetDatabase.IsValidFolder(PARENT_FOLDER + "/" + path);

            if (isFolderExist)
                continue;

            AssetDatabase.CreateFolder(PARENT_FOLDER, path);
        }
    }
}
~~~
If we press **Ctrl+Shift+F12**, The confirm create folders dialog will appears.  
After press "Yes", Each folder name in array "Folders" will create in the Assets folder.  
It will skip an exists one.  

***
## Summary
As you can see, Custom Editor is not as hard as you think.  
The rule is, script need to be in the folder name "Editor".  
In **Unity - Custom Editor Script 101**, We focus on **Custom Menu**.  
There are **Custom Inspector**, **Editor Window** and **Scene View** left.  
We'll discuss those stuff later..  
  
  