---
title: "Unity - Custom Editor 101"
date: 2019-02-01T18:50:52+07:00
draft: false
tags: ["unity", "custom-editor"]
---

## เกริ่นนำ
ถึงแม้ว่า Unity จะมีเครื่องมือพื้นฐานมาให้เราใช้พอสมควร แต่บางทีเราต้องการเครื่องมือที่เฉพาะเจาะจงกับเกมของเรามากๆ เพื่อช่วยให้การพัฒนาเกมของเรา มีความรวดเร็วและสะดวกมากยิ่งขึ้น ซึ่งเราสามารถเพิ่มความสามารถของ Editor ด้วย Custom Editor  

## ใช้ "ExecuteInEditMode" ได้มะ?
**ExecuteInEditMode** attribute เป็นวิธีที่ง่ายที่สุดที่จะทำให้ script ของเราทำงานใน Editor  
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
แต่เราทำอะไรไม่ได้มาก ไม่สามารถปรับแต่งอะไรในส่วนของ UI ใน Editor อย่างเช่น ตัว Inspector ได้
เหมาะกับเวลาที่เราต้องการอยากจะเห็นผลลัพธ์ ตอนอยู่ใน Editor มากกว่าที่จะสร้างเป็นเครื่องมือไว้ใช้เองใน Editor  

## Hello World (Custom Menu)
**Script** จะต้องอยู่ใน folder ที่ชื่อว่า **"Editor"**  
หากยังไม่มี ให้สร้าง folder ขึ้นมาใหม่ก่อน โดยผมจะสร้าง folder ไว้ที่ "Assets/Editor"  
จากนั้นเราจะมาลองทำ Custom Menu, เมื่อเรากดเลือก ให้แสดง Dialog ขึ้นมา  
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
หากเราไปที่ Menu ข้างบนแล้วเลือก "Custom > Greet" ก็จะมี Dialog แสดงขึ้นมาแล้ว, เย้ๆ xD  

## มาทำของที่มีประโยชน์กว่านี้ดีกว่า
ใน Project Game ทุกประเภท อย่างน้อยๆก็ต้องมี folder พวก Script, Scenes, Prefabs, etc..  
งั้นเรามาทำตัวที่ช่วยสร้าง folder พวกนี้กัน
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
พอเรากด **Ctrl+Shift+F12**, ตัว Dialog ก็จะถามเราก่อนที่จะสร้าง folder  
ถ้าเรากด Yes, folder ที่ถูกระบุไว้ใน array "Folders" ก็จะถูกสร้างขึ้น หากยังไม่ได้สร้าง folder นั้นๆไว้ใน Assets  

***
## สรุป
จะเห็นว่าการทำ Custom Editor ไม่ได้ยากอย่างที่คิด  
กฎเหล็กคือ script ต้องอยู่ใน folder ที่ชื่อว่า "Editor"  
สำหรับ **Unity - Custom Editor Script 101** นี้จะเน้นไปที่ **Custom Menu**  
ยังเหลือในส่วนของ **Custom Inspector**, **Editor Window** และในส่วนของ **Scene View** อีก  
เอาไว้คราวหน้า เจอกัน~  
  