'''
Created on Jun 26, 2024

@author: vladk
'''
from tkinter import *
from tkinter import filedialog
from tkinter import ttk
import customtkinter as ctk
import shutil
import os 
import subprocess
from zstd import Zstd
from byml import Byml
from copy import deepcopy
import utils

root = ctk.CTk()
ctk.set_default_color_theme("blue")
ctk.set_appearance_mode("Dark")


global ActorTypeRun
global ActorType
global romfsFolder
ActorType = "aaa"
ActorTypeRun = 0
romfsFolder = ""
DecompressPath = ""
def ActorTypePick(ActorTypeRun):
    
    #various Variables
    Types = ("Armor","Weapon","Item","Unsupported")
    Versions = ("1.2.1","1.2.0","1.1.2","1.1.1","1.1.0")
    global ActorInfo 
    global romfsFolder
    #window setup
    global ActorTypeWin
    ActorTypeWin = ctk.CTkToplevel(root)
    ActorTypeWin.title("Actor Type Picker")
    
    
    
    #dropdown ui
    #i hear people in the walls
    TypeVar = StringVar(ActorTypeWin)
    TypeVar.set("Armor")
    ctk.CTkLabel(ActorTypeWin,text = "Type Of Actor:").grid(row=3, column = 0, sticky =W)
    global ActorTypeEnt
    ActorTypeEnt = ctk.CTkComboBox(ActorTypeWin,values = Types)
    ActorTypeEnt.grid(row=3, column = 1, sticky =W)
    
    VersionVar = StringVar(ActorTypeWin)
    VersionVar.set("1.2.1")
    ctk.CTkLabel(ActorTypeWin,text = "Totk Version: ").grid(row=4, column = 0, sticky =W)
    global VersionEnt
    VersionEnt = ctk.CTkComboBox(ActorTypeWin,values = Versions)
    VersionEnt.grid(row=4, column = 1, sticky =W)

    #file picking
    
    def Check():
        global Vers
        global PouchZsPath
        global GameZsPath
        global ActorZsPath
        global TagZsPath
        global romfsFolder 
        global Vers
        Vers = VersionEnt.get()
        NoError = True
        if Vers == "1.2.1": Vers = ".121."
        if Vers == "1.2.0": Vers = ".120."
        if Vers == "1.1.2": Vers = ".112."
        if Vers == "1.1.1": Vers = ".111."
        if Vers == "1.1.0": Vers = ".110."
        if romfsFolder == "":
            NoError = False
            Error("NoRomfsSubmit")
        if NoError:
            try:
                PouchZsPath = romfsFolder + "/RSDB/PouchActorInfo.Product"+Vers+"rstbl.byml.zs"
                GameZsPath = romfsFolder + "/RSDB/GameActorInfo.Product"+Vers+"rstbl.byml.zs"
                ActorZsPath = romfsFolder + "/RSDB/ActorInfo.Product"+Vers+"rstbl.byml.zs"
                TagZsPath = romfsFolder + "/RSDB/Tag.Product"+Vers+"rstbl.byml.zs"
                RsdbFolderPath = romfsFolder+"/RSDB"
                ZsBymlOutputPath = romfsFolder +"/RSDB"
            
            except(NameError,FileNotFoundError):
                Error("NoRomfsSubmit")
                NoError = False
                
        
                
        if NoError:
            try: 
                if os.path.exists(RsdbFolderPath) == False:
                    os.makedirs(RsdbFolderPath)
                    print("BRO WHAT WHY DO YOU NOT HAVE A RSDB FILE (enter multiple sob emojis here, like 5 or 6)")
                if os.path.exists(PouchZsPath) == False:
                    shutil.copy(DecompressPath+"/RSDB/PouchActorInfo.Product"+Vers+"rstbl.byml.zs", ZsBymlOutputPath)
                    print("tsktsk1")
                if os.path.exists(GameZsPath) == False:
                    shutil.copy(DecompressPath+"/RSDB/GameActorInfo.Product"+Vers+"rstbl.byml.zs", ZsBymlOutputPath)
                    print("tsktsk2")
                if os.path.exists(ActorZsPath) == False:
                    shutil.copy(DecompressPath+"/RSDB/ActorInfo.Product"+Vers+"rstbl.byml.zs", ZsBymlOutputPath)
                    print("tsktsk3")
                if os.path.exists(TagZsPath) == False:
                    shutil.copy(DecompressPath+"/RSDB/Tag.Product"+Vers+"rstbl.byml.zs", ZsBymlOutputPath)
                    print("tsktsk3")
                print(Vers)
                decompressZs = Zstd(DecompressPath)
                print("selected Folder")
                PouchByml = decompressZs.Decompress(PouchZsPath, output_dir=ZsBymlOutputPath, with_dict=True, no_output=False)
                GameByml= decompressZs.Decompress(GameZsPath, output_dir=ZsBymlOutputPath, with_dict=True, no_output=False)
                ActorByml= decompressZs.Decompress(ActorZsPath, output_dir=ZsBymlOutputPath, with_dict=True, no_output=False)
                TagByml= decompressZs.Decompress(TagZsPath, output_dir=ZsBymlOutputPath, with_dict=True, no_output=False)
                print("Decompressed")
                global PouchdecompressByml
                PouchdecompressByml = Byml(PouchByml, filename="PouchActorInfo.Product"+Vers+"rstbl.byml")
                global GamedecompressByml
                GamedecompressByml = Byml(GameByml, filename="GameActorInfo.Product"+Vers+"rstbl.byml")
                global ActordecompressByml
                ActordecompressByml = Byml(ActorByml, filename="ActorInfo.Product"+Vers+"rstbl.byml")
                global TagdecompressByml
                TagdecompressByml = Byml(TagByml, filename="/RSDB/Tag.Product"+Vers+"rstbl.byml.zs")
                print("Set Variables to Byml's succefully")
                global PouchActorInfo
                PouchActorInfo = PouchdecompressByml.root_node
                global GameActorInfo
                GameActorInfo = GamedecompressByml.root_node
                global ActorInfo
                ActorInfo = ActordecompressByml.root_node
                global TagInfo
                TagInfo = TagdecompressByml.root_node   
                print(type(TagInfo))
                print("Rsdb files have been set properly :))))")
                dumpFolderEnt.delete(0,END)
                dumpFolderEnt.insert(0,DecompressPath)
                WindowChange()
            except:
                Error("WrongVersion")
    def OpenRomfsFolder():
        global romfsFolder        
        romfsFolder = filedialog.askdirectory(title = "Select Your romfs MOD folder")       
        romfFolderEnt.delete(0,END)
        romfFolderEnt.insert(0,romfsFolder)
        
    def OpenRomfsDumpFolder():
        global DecompressPath
        DecompressPath = filedialog.askdirectory(title = "Select Your romfs DUMP folder")
        dumpFolderEnt.delete(0,END)
        dumpFolderEnt.insert(0,DecompressPath)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              
    
        
        
    #they are whispering things to me
    
    #Pouch Actor layout

    
    ctk.CTkLabel (ActorTypeWin,text = "romfs Folder").grid(row=0, column = 0, sticky =W)
    romfFolderEnt = ctk.CTkEntry(ActorTypeWin,width = 225)
    romfFolderEnt.grid(row = 0,column = 1, sticky = W)
    ctk.CTkButton(ActorTypeWin, text= "Pick Folder",width = 10, command = OpenRomfsFolder).grid(row=0,column =2, sticky = W)
    romfFolderEnt.delete(0,END)
    romfFolderEnt.insert(0,romfsFolder)
    
    
    ctk.CTkLabel (ActorTypeWin,text = "romfs dump Folder  ").grid(row=1, column = 0, sticky =W)
    dumpFolderEnt = ctk.CTkEntry(ActorTypeWin,width = 225)
    dumpFolderEnt.grid(row = 1,column = 1, sticky = W)
    ctk.CTkButton(ActorTypeWin, text= "Pick Folder",width = 10, command = OpenRomfsDumpFolder).grid(row=1,column =2, sticky = W)
    dumpFolderEnt.delete(0,END)
    dumpFolderEnt.insert(0,DecompressPath)
    
    
    #Submit Button, moves on to the Armor thing for now cuz i havent set the other one up yet
    ctk.CTkButton(ActorTypeWin, text= "Submit",width = 6, command = Check).grid(row=3,column =2, sticky = W) 
    
    #disclamer
    Disc = ctk.CTkTextbox(ActorTypeWin, width = 400, height = 65)
    Disc.grid(row = 5, column =0, columnspan = 3, sticky = W)
    DiscText = "Disclamer, The Unsupported option is UNSUPPORTED\nIts likely that it will generate entries without failing or messing up\nbut again, it is unsupported, so use with caution"
    Disc.insert(0.0,DiscText)
    Disc.configure(state = "disabled")
   

    
    if ActorTypeRun == 0: 
        ActorTypeWin.mainloop()
    else:
        ActorTypeWin.quit()
#the shadows whisper things to me
def Error(ErrorType):
    Err = ctk.CTkToplevel(root)
    Err.title("WompWomp you got an error! hahahhaha loser imagine getting an error")
    def Ok():
        Err.destroy()
    
    if ErrorType == "PouchFileNotFound":
        ctk.CTkLabel(Err,text = "Pouch Actor txt not found! \nmake sure its named PouchActorInfo.txt!").grid(row=0,column =0, sticky = W)
        
    elif ErrorType == "GameFileNotFound":
        ctk.CTkLabel(Err,text = "Game Actor txt not found! \nmake sure its named GameActorInfo.txt!").grid(row=0,column =0, sticky = W)
        
    elif ErrorType == "ActorFileNotFound":
        ctk.CTkLabel(Err,text = "Actor txt not found! \nmake sure its named ActorInfo.txt!").grid(row=0,column =0, sticky = W)
    #PouchRowIDNotFound
    elif ErrorType == "PouchRowIDNotFound":
        ctk.CTkLabel(Err,text = "Replaced Actor ID not found in Pouch Actor! \nMake sure you spelled it correctly").grid(row=0,column =0, sticky = W)
        
    elif ErrorType == "GameRowIDNotFound":
        ctk.CTkLabel(Err,text = "Replaced Actor ID not found in Game Actor! \nMake sure you spelled it correctly").grid(row=0,column =0, sticky = W)    
    
    elif ErrorType == "ActorRowIDNotFound":
        ctk.CTkLabel(Err,text = "Replaced Actor ID not found in Actor! \nMake sure you spelled it the ID correctly").grid(row=0,column =0, sticky = W)
        
    elif ErrorType == "EmptyEntry":
        ctk.CTkLabel(Err,text = "One of the needed Entry's are empty! \nDouble check them!!").grid(row=0,column =0, sticky = W)
        
    elif ErrorType == "DoubleID":
        ctk.CTkLabel(Err,text = "Whoops! It seems your Standalone ID already has an entry!\nTry deleting the old entry or think of a new one!").grid(row=0,column =0, sticky = W)
        
    elif ErrorType == "NoRomfsSubmit":
        ctk.CTkLabel(Err,text = "Dude... You didn't even select your mod folder/right version... ").grid(row=0,column =0, sticky = W)  
        ctk.CTkLabel(Err,text = "Ive been trying my best to make this as user friendly as possible").grid(row=1,column =0, sticky = W)
        ctk.CTkLabel(Err,text = "to make this tool super easy to use, and you didnt even select your mod romfs folder/the right version??? ").grid(row=2,column =0, sticky = W)     
        ctk.CTkLabel(Err,text = "I feel bad that i even have to make this error window, i had to spend MY TIME making THIS ERROR WINDOW").grid(row=3,column =0, sticky = W)  
        ctk.CTkLabel(Err,text = "BECAUSE YOUR TOO DUMB TO SELECT A OPTION THATS CLEARLY LABELED").grid(row=4,column =0, sticky = W)                   
    
    elif ErrorType == "WrongVersion":
        ctk.CTkLabel(Err,text = "Hey! looks like you chose the wrong version!\n Look in your dumpromfs/RSDB folder and check for numbers inside the rsdb files, thats your version!").grid(row=0,column =0, sticky = W)     
        ctk.CTkLabel(Err,text = "or maybe you didnt select anything for the file inputs...").grid(row=1,column =0, sticky = W)
        
    ctk.CTkLabel(Err,text = "Error: "+ErrorType).grid(row=5,column =0, sticky = W)
    ctk.CTkButton(Err, text= "Ok",width = 3, command = Ok).grid(row=6,column =0, sticky = W)
    
    
#there are people in my walls 
# and they want to get out
def WindowChange():
    global ActorTypeWin
    global WindowOpen
    global ActorTypeRun
    global ActorType
    ActorType = ActorTypeEnt.get()
    
    if ActorTypeRun != 0:
        ActorTypeRun = 1
    
    if(ActorTypeEnt.get() == "Armor"):
        ActorTypeRun = 1
        
    elif(ActorTypeEnt.get() == "Unsupported"):
        ActorTypeRun = 2
        
    elif(ActorTypeEnt.get() == "Weapon"):
        ActorTypeRun = 3   
    
    elif(ActorTypeEnt.get() == "Item"):
        ActorTypeRun = 4  
       
    if ActorTypeRun != 0:
        ActorTypeWin.destroy()
        
    
    if(ActorTypeRun == 0):
        ActorTypeWin(ActorTypeRun)
        
    elif(ActorTypeRun == 1):
        ArmorStandalone(ActorTypeRun)
        
    elif(ActorTypeRun == 2):
        UnsupportedStandalone(ActorTypeRun)
        
    elif(ActorTypeRun == 3):
        WeaponStandalone(ActorTypeRun)
        
    elif(ActorTypeRun == 4):
        ItemStandalone(ActorTypeRun)
def RSDB():
    global ActorTypeRun
    StandaloneActorID = StandaloneActorIDEnt.get()
    searchInp = searchInpEnt.get()
    Var1 = "-1"
    Var2 = "-1"
    Var3 = "-1"
    Effect1 = "-1"
    Effect2 = "-1"
    Effect3 = "-1"
    if ActorTypeRun == 1 and DefenseEnt.get() != "":
        Var1 = utils.s32(int(DefenseEnt.get()))

    if ActorTypeRun == 3 and DamageEnt.get() != "":
        Var1 = utils.s32(int(DamageEnt.get()))
        
    if ActorTypeRun == 3 and DurabilityEnt.get() != "":
        Var2 = utils.s32(int(DurabilityEnt.get()))
    
    if ActorTypeRun == 4 and (EffectsEnt.get() != "Default" or EffectsEnt.get() != "None"):
        Effect1 = EffectsEnt.get()
        Effect2 = utils.s32(int(LevelsEnt.get()))
        if DurationEnt.get() != "":
            Effect3 = utils.s32(int(DurationEnt.get()))
    #Effect3 = utils.s32(int(DurationEnt.get()))
    
    
    if ActorTypeRun == 4 and EffectsEnt.get() == "Default":
        print("btw when selecting Default it keeps all of it the same lol")
    if ActorTypeRun == 4 and EffectsEnt.get() == "None":
        print("btw when selecting Default it keeps all of it the same lol")
        
    searchInp 
    GameSearch = "__RowId: " + searchInp
    ActorSearch = "  __RowId: " + searchInp +"\n"
    #pouch actor stuff
    global PouchRsdbOutput
    PouchRsdbOutput = "\nTHIS IS THE POUCH ACTOR INFO \n"
    #game actor stuff
    global GameRsdbOutput
    GameRsdbOutput = "\nTHIS IS THE GAME ACTOR INFO \n"
    #actorinfo actor stuff
    global ActorRsdbOutput
    ActorRsdbOutput = "\nTHIS IS THE ACTOR ACTOR INFO \n"
    #i, pretty simple, just a throwaway var
    i=0
    #there are bugs in my skin
    PouchActorRSDB(PouchRsdbOutput,searchInp,StandaloneActorID,ActorType,PouchActorInfo,Var1,Effect1,Effect2,Effect3)
    GameActorRSDB(GameRsdbOutput,searchInp,StandaloneActorID,ActorType,GameActorInfo,Var2)
    ActorRSDB(ActorRsdbOutput,searchInp,StandaloneActorID,ActorType,ActorInfo,Var1)
    
    Output()
    
def Output():
    OUTPUT = ctk.CTkToplevel(root)
    OUTPUT.title("Output duhh")
    

    global Progress
    global romfsFolder
    global DecompressPath
    global Vers

    global PouchZsPath
    global PouchdecompressByml
    global PouchActorInfo
    global PouchStandaloneDict
    global PouchdecompressByml
                
    global GameZsPath
    global GamedecompressByml
    global GameActorInfo
    global GameStandaloneDict
    global GamedecompressByml
    
    global ActorZsPath
    global ActordecompressByml
    global ActorInfo
    global ActorStandaloneDict
    global ActordecompressByml
    
    IsThereNoError = True
    print("globals!")
    try:
        PouchActorInfo.append(PouchRsdb)
        print("yay1")
    except(NameError):
        IsThereNoError = False
        print("aww1")
        
    try:
        GameActorInfo.append(GameRsdb)
        print("yay2")
    except(NameError):
        IsThereNoError = False
        print("aww2")
        
    try:
        ActorInfo.append(ActorRsdb)
        print("yay3")
    except(NameError):
        IsThereNoError = False
        print("aww3")
    #PouchActorInfo = PouchdecompressByml.root_node
    if IsThereNoError:        
        compressByml = Zstd(DecompressPath)
        print("compressByml has been init")
        PouchdecompressByml.root_node = PouchActorInfo
        PouchCompressedByml = PouchdecompressByml.Reserialize(romfsFolder +"/RSDB")
        compressByml._CompressFile( romfsFolder +"/RSDB/PouchActorInfo.Product"+Vers+"rstbl.byml", output_dir=romfsFolder +"/RSDB", level=16, with_dict=False)
        
        print("Pouch has been compressed")
        
        GamedecompressByml.root_node = GameActorInfo
        GameCompressedByml = GamedecompressByml.Reserialize(romfsFolder +"/RSDB")
        compressByml._CompressFile( romfsFolder +"/RSDB/GameActorInfo.Product"+Vers+"rstbl.byml", output_dir=romfsFolder +"/RSDB", level=16, with_dict=False)
        
        print("Game has been compressed")
        print("The Actor File WILL take a while to compress, for me it finished within 20sec but give it abt a minute depending on your specs")
        ActordecompressByml.root_node = ActorInfo
        ActorCompressedByml = ActordecompressByml.Reserialize(romfsFolder +"/RSDB")
        compressByml._CompressFile( romfsFolder +"/RSDB/ActorInfo.Product"+Vers+"rstbl.byml", output_dir=romfsFolder +"/RSDB", level=16, with_dict=False)
        
        print("Actor has been compressed")
        
        
        print("YAYYY")
    
    
    ctk.CTkLabel(OUTPUT, text = "Finished!").grid(row = 0,column = 0, sticky = W)
    OUTPUT.mainloop()
    
#the function

#calls the Pouch actor function

def PouchActorRSDB(PouchRsdbOutput,search,StandaloneActorID,ActorType,PouchActorInfo,Var1,Effect1,Effect2,Effect3):
    PouchFoundActor = 0   
    NoError = True
    NoDoubleID = True
    i=0
    
    try:
        while (PouchFoundActor == 0):
            if (PouchActorInfo[i]["__RowId"] == StandaloneActorID):
                PouchFoundActor = 1    
                NoError = False
                NoDoubleID = False
            i+=1
    except IndexError:
        print("good job! you dont have repeating ID's!")
    
    i = 0
    #searches for the row id thing
    try:
        while (PouchFoundActor == 0):
            if (PouchActorInfo[i]["__RowId"] == search and NoDoubleID == True):
                PouchStandaloneDict = deepcopy(PouchActorInfo[i])
                PouchFoundActor = 1               
                PouchStandaloneDict.update({'__RowId': StandaloneActorID})
                
            i+=1
    except IndexError:
        Error("PouchRowIDNotFound")
        NoError = False
    
    if NoError:
        
        if (ActorType == "Armor" and Var1 != "-1" and NoDoubleID):        
            PouchStandaloneDict.update({'EquipmentPerformance': Var1})
    
        if (ActorType == "Weapon" and Var1 != "-1" and NoDoubleID):        
            PouchStandaloneDict.update({'EquipmentPerformance': Var1})
        
        try: 
            if (ActorType == "Item" and Effect1 == "None" and NoDoubleID):        
                poplol = PouchStandaloneDict.pop({'CureEffectType',"womp"})
                poplol = PouchStandaloneDict.pop({'CureEffectLevel',"womp"})
                poplol = PouchStandaloneDict.pop({'CureEffectiveTime',"womp"})
        except:
            print('idk womp womp, something to do with the customizations you made, prolly the item tbh, mad buggy cuz i havent tested it much lol')
        
        if (ActorType == "Item" and (Effect1 != "-1" and Effect1 != "None" and Effect1 != "Default") and NoDoubleID):        
            PouchStandaloneDict.update({'CureEffectType': Effect1})
        
        if (ActorType == "Item" and (Effect1 != "None" or Effect1 != "Default") and Effect2 != "-1" and NoDoubleID):        
            PouchStandaloneDict.update({'CureEffectLevel': Effect2})
        
        if (ActorType == "Item" and (Effect1 != "None" or Effect1 != "Default") and Effect3 != "-1" and NoDoubleID):        
            PouchStandaloneDict.update({'CureEffectiveTime': Effect3})
        
        
    if NoDoubleID == False:
        Error("DoubleID")    
    
    if NoError:
        global PouchRsdb
        PouchRsdb = PouchStandaloneDict
   

##- {__RowId: Armor_1038_Upper}
##- {HasAutoAnim: true,__RowId: Armor_1039_Head}
i = 0
def GameActorRSDB(GameRsdbOutput,search,StandaloneActorID,ActorType,GameActorInfo,Var1):
    GameFoundActor = 0
    NoError = True
    i=0
    #searches for the row id thing
    try:
        while (GameFoundActor == 0):
            if (GameActorInfo[i]["__RowId"] == search):
                GameStandaloneDict = deepcopy(GameActorInfo[i])
                GameFoundActor = 1               
                GameStandaloneDict.update({'__RowId': StandaloneActorID})
            i+=1
    except IndexError:
        Error("GameRowIDNotFound")
        NoError = False
    
    if NoError: 
        if (ActorType == "Weapon" and Var1 != "-1"):        
            GameStandaloneDict.update({'MaxLife': Var1})
    
    if NoError:
        GameRsdbOutput = GameStandaloneDict
        global GameRsdb
        GameRsdb = GameStandaloneDict


#actor function
def ActorRSDB(ActorRsdbOutput,search,StandaloneActorID,ActorType,ActorInfo,Var1):
    ActorFoundActor = 0
    NoError = True
    NoDoubleID = True

    i=0
    #searches for the row id thing
    try:
        while (ActorFoundActor == 0):
            if (ActorInfo[i]["__RowId"] == StandaloneActorID):
                ActorFoundActor = 1    
                NoError = False
                NoDoubleID = False
            i+=1
    except IndexError:
        print("good job! you dont have repeating ID's!")
    
    i = 0
    #searches for the row id thing
    try:
        while (ActorFoundActor == 0):
            if (ActorInfo[i]["__RowId"] == search and NoDoubleID == True):
                ActorStandaloneDict = deepcopy(ActorInfo[i])
                ActorFoundActor = 1               
                ActorStandaloneDict.update({'__RowId': StandaloneActorID})
                ActorStandaloneDict.update({'ActorName': StandaloneActorID})
                
            i+=1
    except IndexError:
        Error("ActorRowIDNotFound")
        NoError = False
    
    if NoError:
        ActorRsdbOutput = ActorStandaloneDict
        global ActorRsdb
        ActorRsdb = ActorStandaloneDict
#calls the Pouch actor function

def UnsupportedStandalone(ActorTypeRun):
    global i
    i = 0
    global Un
    Un = ctk.CTkToplevel(root)
    Un.title("Standalone RSDB editor v0.5 Unsupported type (Might give false results!)")
    def Clear():
        StandaloneActorIDEnt.delete(0,END)
        searchInpEnt.delete(0,END)
    def Insert():
        StandaloneActorIDEnt.insert(0,"asdsadasd")
        searchInpEnt.insert(0,"Weapon_Sword_070")
    def Home():
        global ActorTypeRun
        ActorTypeRun = 0
        Un.destroy()
        ActorTypePick(ActorTypeRun)
    
    def Check():
        if StandaloneActorIDEnt.get() != "" and searchInpEnt.get() != "":
            RSDB()
        else:
            Error("EmptyEntry")        
    ctk.CTkLabel (Un,text = "Standalone Actor ID:").grid(row=0, column = 2, sticky =W)
    ctk.CTkLabel (Un,text = "Actor You Replaced:").grid(row=0, column = 0, sticky =W)
    
    global StandaloneActorIDEnt
    global searchInpEnt
    StandaloneActorIDEnt = ctk.CTkEntry(Un,width = 175)
    StandaloneActorIDEnt.grid(row = i,column = 3, sticky = W)
    
    searchInpEnt = ctk.CTkEntry(Un,width = 150)
    searchInpEnt.grid(row = i,column = 1, sticky = W)
    
    
    

    ctk.CTkButton(Un, text= "Make the Edits!!!",width = 17, command = Check).grid(row=99,column =0, sticky = W)
    ctk.CTkButton(Un, text= "Clear",width = 17, command = Clear).grid(row=99,column =3, sticky = W)
    ctk.CTkButton(Un, text= "Quick Insert",width = 17, command = Insert).grid(row=99,column =1, sticky = W)
    ctk.CTkButton(Un, text= "Home",width = 17, command = Home).grid(row=99,column =9, sticky = W)
    
    if(ActorTypeRun == 1):
        Un.mainloop()
        
    elif(ActorTypeRun == 2):
        Un.mainloop()
    else:
        return ActorTypeRun

def ItemStandalone(ActorTypeRun):
    global i
    i = 0
    global Item
    Item = ctk.CTkToplevel(root)
    Item.title("Item Standalone RSDB editor v0.5")
    
    
    Effects = ("Default","None", "LifeMaxUp","StaminaRecover","ExStaminaMaxUp","ResistHot","ResistElectric","ResistElectric"
               ,"AllSpeed","AttackUp","DefenseUp","QuietnessUp","ResistBurn","LifeRepair","LightEmission"
               ,"NotSlippy","SwimSpeedUp","SwimSpeedUp","AttackUpCold","AttackUpHot","AttackUpThunderstorm","MiasmaGuard")
    
    Levels = ("3","2","1")
    def Clear():
        StandaloneActorIDEnt.delete(0,END)
        searchInpEnt.delete(0,END)
    def Insert():
        StandaloneActorIDEnt.insert(0,"asdsadasd")
        searchInpEnt.insert(0,"Item_Enemy_24")
    def Home():
        global ActorTypeRun
        ActorTypeRun = 0
        Item.destroy()
        ActorTypePick(ActorTypeRun)
    def Check():
        if StandaloneActorIDEnt.get() != "" and searchInpEnt.get() != "":
            RSDB()
        else:
            Error("EmptyEntry")
            
    ctk.CTkLabel (Item,text = "Standalone Actor ID:").grid(row=0, column = 2, sticky =W)
    ctk.CTkLabel (Item,text = "Actor You Replaced:").grid(row=0, column = 0, sticky =W)
    ctk.CTkLabel (Item,text = "Item Effect:").grid(row=1, column = 2, sticky =W)
    ctk.CTkLabel (Item,text = "Level of Effect:").grid(row=1, column = 0, sticky =W)
    ctk.CTkLabel (Item,text = "Effect Duration:").grid(row=1, column = 4, sticky =W)
    
    
    global StandaloneActorIDEnt
    global searchInpEnt
    global PriceEnt
    global EffectsEnt
    global LevelsEnt
    global DurationEnt
    
    
    StandaloneActorIDEnt = ctk.CTkEntry(Item,width = 175)
    StandaloneActorIDEnt.grid(row = i,column = 3, sticky = W)
    
    searchInpEnt = ctk.CTkEntry(Item,width = 150)
    searchInpEnt.grid(row = i,column = 1, sticky = W)
    
    
    EffectsEnt = ctk.CTkComboBox(Item,values = Effects)
    EffectsEnt.grid(row=1, column = 1, sticky =W)

    LevelsEnt = ctk.CTkComboBox(Item,values = Levels)
    LevelsEnt.grid(row=1, column = 3, sticky =W)
    
    DurationEnt = ctk.CTkEntry(Item,width = 150)
    DurationEnt.grid(row = 1,column = 5, sticky = W)
    
    ctk.CTkButton(Item, text= "Make the Edits!!!",width = 17, command = Check).grid(row=99,column =0, sticky = W)
    ctk.CTkButton(Item, text= "Clear",width = 17, command = Clear).grid(row=99,column =3, sticky = W)
    ctk.CTkButton(Item, text= "Quick Insert",width = 17, command = Insert).grid(row=99,column =1, sticky = W)
    ctk.CTkButton(Item, text= "Home",width = 17, command = Home).grid(row=99,column =9, sticky = W)
    
    if(ActorTypeRun == 4):
        Item.mainloop()

    else:
        return ActorTypeRun


def WeaponStandalone(ActorTypeRun):
    global i
    i = 0
    global Weapon
    Weapon = ctk.CTkToplevel(root)
    Weapon.title("Standalone RSDB editor v0.5")
    def Clear():
        StandaloneActorIDEnt.delete(0,END)
        searchInpEnt.delete(0,END)
        DamageEnt.delete(0,END)
    def Insert():
        Clear()
        StandaloneActorIDEnt.insert(0,"MasterSword?????????")
        searchInpEnt.insert(0,"Weapon_Sword_070")
        DamageEnt.insert(0,"69")
        DurabilityEnt.insert(0,"420")
    def Home():
        global ActorTypeRun
        ActorTypeRun = 0
        Weapon.destroy()
        ActorTypePick(ActorTypeRun)
    
    def Check():
        if StandaloneActorIDEnt.get() != "" and searchInpEnt.get() != "":
            RSDB()
        else:
            Error("EmptyEntry")
     
    ctk.CTkLabel (Weapon,text = "Weapon You Replaced:").grid(row=0, column = 0, sticky =W)   
    ctk.CTkLabel (Weapon,text = "Standalone Actor ID:").grid(row=0, column = 2, sticky =W)
    
    ctk.CTkLabel (Weapon,text = "Damage: ").grid(row=1, column = 0, sticky =W)    
    ctk.CTkLabel (Weapon,text = "Durability: ").grid(row=1, column = 2, sticky =W) 
    
    
    global StandaloneActorIDEnt
    global searchInpEnt
    global DamageEnt
    global DurabilityEnt
    
    StandaloneActorIDEnt = ctk.CTkEntry(Weapon,width = 175)
    StandaloneActorIDEnt.grid(row = 0,column = 3, sticky = W)
    
    searchInpEnt = ctk.CTkEntry(Weapon,width = 150)
    searchInpEnt.grid(row = 0,column = 1, sticky = W)
    
    DamageEnt = ctk.CTkEntry(Weapon,width = 75)
    DamageEnt.grid(row = 1,column = 1, sticky = W)
    
    DurabilityEnt = ctk.CTkEntry(Weapon,width = 75)
    DurabilityEnt.grid(row = 1,column = 3, sticky = W)
    
    
    ctk.CTkButton(Weapon, text= "Make the Edits!!!",width = 17, command = Check).grid(row=99,column =0, sticky = W)
    ctk.CTkButton(Weapon, text= "Clear All",width = 17, command = Clear).grid(row=99,column =3, sticky = W)
    ctk.CTkButton(Weapon, text= "Quick Insert",width = 17, command = Insert).grid(row=99,column =1, sticky = W)
    ctk.CTkButton(Weapon, text= "Home",width = 17, command = Home).grid(row=99,column =9, sticky = W)
        
    if(ActorTypeRun == 3):
        Weapon.mainloop()
    else:
        return ActorTypeRun
    

def ArmorStandalone(ActorTypeRun):
    global i
    i = 0
    global Armor
    Armor = ctk.CTkToplevel(root)
    Armor.title("Standalone RSDB editor v0.5")
    def Clear():
        StandaloneActorIDEnt.delete(0,END)
        searchInpEnt.delete(0,END)
        DefenseEnt.delete(0,END)
    def Insert():
        Clear()
        StandaloneActorIDEnt.insert(0,"asdsadasd")
        searchInpEnt.insert(0,"Armor_001_Head")
    def Home():
        global ActorTypeRun
        ActorTypeRun = 0
        Armor.destroy()
        ActorTypePick(ActorTypeRun)
      
    def Check():
        if StandaloneActorIDEnt.get() != "" and searchInpEnt.get() != "":
            RSDB()
        else:
            Error("EmptyEntry")
        
        
    ctk.CTkLabel (Armor,text = "Standalone Actor ID:").grid(row=0, column = 2, sticky =W)
    ctk.CTkLabel (Armor,text = "Actor You Replaced:").grid(row=0, column = 0, sticky =W)
    ctk.CTkLabel (Armor,text = "Defense").grid(row=0, column = 4, sticky =W)
    
    global StandaloneActorIDEnt
    global searchInpEnt
    global DefenseEnt
    StandaloneActorIDEnt = ctk.CTkEntry(Armor,width = 175)
    StandaloneActorIDEnt.grid(row = i,column = 3, sticky = W)
    
    searchInpEnt = ctk.CTkEntry(Armor,width = 150)
    searchInpEnt.grid(row = i,column = 1, sticky = W)
    
    DefenseEnt = ctk.CTkEntry(Armor,width = 75)
    DefenseEnt.grid(row = 0,column = 5, sticky = W)
    
    ctk.CTkButton(Armor, text= "Make the Edits!!!",width = 17, command = Check).grid(row=99,column =0, sticky = W)
    ctk.CTkButton(Armor, text= "Clear All",width = 17, command = Clear).grid(row=99,column =3, sticky = W)
    ctk.CTkButton(Armor, text= "Quick Insert",width = 17, command = Insert).grid(row=99,column =1, sticky = W)
    ctk.CTkButton(Armor, text= "Home",width = 17, command = Home).grid(row=99,column =9, sticky = W)
        
    if(ActorTypeRun == 1):
        Armor.mainloop()
        
    elif(ActorTypeRun == 2):
        Armor.mainloop()
    else:
        return ActorTypeRun
    
    
if __name__ == "__main__":
    if(ActorTypeRun == 0):
        ActorTypePick(ActorTypeRun)
    if(ActorTypeRun == 1):
        ArmorStandalone(ActorTypeRun)
    if(ActorTypeRun == 2):
        UnsupportedStandalone(ActorTypeRun)
    if(ActorTypeRun == 3):
        WeaponStandalone(ActorTypeRun)
    if(ActorTypeRun == 4):
        ItemStandalone(ActorTypeRun)


#if you saw this your gay, dont say anything, just say "the comment at the top is killing me" or smth, trick the others













#help me




        
#there are people watching me



#apparently some of yall look here, well guess what
#the cotton candy in your walls went bad!
#i tried it and it made my throat itchy
#not nice man
#oh and im in your walls watching you sleep, you talk in your sleep btw, really annoying





#there are bugs in my skin and i want to dig them out


#gate keeping the source code so your eyes dont have to see the abomination that is my code lmaooo






#i am in severe mental distress
