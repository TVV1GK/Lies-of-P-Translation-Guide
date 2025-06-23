# Translation Guide for Lies of P

## 1. Export Localization Files

1. Download [FModel](https://fmodel.app/)
	- This is a program that can read Unreal Engine's .pak files and extract their contents.
2. Open `FModel.exe`
	- If this isn't your first time using this program, click on **Directory > Selector**
3. Configure the settings as follows:
	- **Detected Game**: Ignore this field (it will update automatically after clicking **OK**)
	- **Directory**: Depending on your game version, set the directory to:
		- Steam: `...\Steam\steamapps\common\Lies of P\LiesofP\Content\Paks`
		- Game Pass: `...\XboxGames\Lies of P\Content\LiesofP\Content\Paks`
	- **UE Versions**: `GAME_UE4_27`
4. Click **OK**
5. If it shows that a new update is available, I recommend clicking **Download Latest Release**
6. Currently, you cannot access any of the `.pak` files. To access them, navigate to [this page (FearLess Cheat Engine)](https://fearlessrevolution.com/viewtopic.php?t=25815#:~:text=Directory%20>%20AES), find and copy the *AES key*, then paste it into **Directory > AES** in *FModel*. After this, everything should appear green, indicating they're accessible.
7. Double-click on `pakchunk0-WindowsNoEditor.pak` and navigate to `LiesofP/Content/Localization/LOP_Game/en/`
	- You can choose any other language to overwrite
8. Right-click on `LOP_Game.locres` and select **Export Raw Data (.locres)**
	- These are the text strings found in the game, with some important notes:
		- The main language is `ko` (Korean), meaning what you find in other language files will be present here, but not vice versa. For example, language names are only set here, so if you want to see your language's name in the game, you need to export this as well (the same applies to advanced graphics settings like *NVIDIA DLSS Frame Generation*).
		- Special texts like "Lie Or Die" and "Lost Ergo Recovered" are not actually text — they are images. Every language uses the English version by default. I only recommend translating these if you're confident you can create an image with the same font and size as the original. Otherwise, it will look out of place. If you want to translate these, see [Special Texts](#special-texts) now.
9. Next to `FModel.exe`, there should be an `Output\Exports\` folder. Navigate there and copy its contents to another location.

## 2. Convert Unreal Engine's Localization File to a Readable Format

1. Download [UnrealLocres](https://github.com/akintos/UnrealLocres/releases/latest)
2. Place `UnrealLocres.exe` in an empty folder and copy `LOP_Game.locres` into the same folder
3. Open Command Prompt in that folder (you can do this by right-clicking in the folder and selecting **Open in Windows Terminal**)

Choose which file type you want to use for translating: [`.pot`](#pot) (recommended) or [`.csv`](#csv)

#### `.pot`
4. Run the command: `UnrealLocres.exe export LOP_Game.locres -f pot`
5. You should now have a `LOP_Game.pot` file. This is a template file for translation.
6. It's time to start translating! Find a program that can read `.pot` files.
	- You can use [Poedit](https://poedit.net/), which is free and user-friendly translation software.
	- Save your work as a `.po` file. This will be needed for converting back to `.locres`.
	- Understanding the formatting:
		- `\n` - Manual line break
		- `<Style_Goes_Here>[Translatable text]</>` - Only translate the `[Translatable text]` part. If there's no `</>`, you should still leave the `<Style_Goes_Here>` part unchanged.
		- `\"` - *Poedit* handles this, so you aren't using that (unless you're looking directly at the `LOP_Game.pot` or your `.po` file). It's used for quotation marks so the program knows it's not the end of the text.
	- If you exported the `ko` language:
		- Copy and paste any additional entries you want to translate from `LOP_Game.pot` (the `ko` version) to your `.po` file
		- To change the language name: Search for `GameString_Culture`
		- To change advanced graphics settings: Search for what you see in-game when the language is set to English, for example `NVIDIA DLSS Frame Generation`
		- To change the HP (Health Points) text: Search for `GameString_HealthPoint`
		- You should not translate text whose keys contain *GameString_BGM*
7. When you've finished or want to test how your work looks, place your `.po` file in the same folder as `UnrealLocres.exe`
8. Open Command Prompt in that folder
9. Run the command: `UnrealLocres.exe import LOP_Game.locres YOUR_PO_SAVE_FILE.po -f po`
10. If everything went well, you should have a `LOP_Game.locres.new` file in the folder. This is your translated localization file.
11. Copy it to the `LiesofP\Content\Localization\LOP_Game\en\` folder (or the language you're overwriting), delete the original `LOP_Game.locres`, and remove the `.new` extension from the new file.

#### `.csv`
4. Run the command: `UnrealLocres.exe export LOP_Game.locres -f csv`
5. You should now have a `LOP_Game.csv` file.
6. It's time to start translating! Open the file in a spreadsheet program.
	- You can use *Google Sheets*.
	- Understanding the formatting:
		- `<Style_Goes_Here>[Translatable text]</>` - Only translate the `[Translatable text]` part. If there's no `</>`, you should still leave the `<Style_Goes_Here>` part unchanged.
		- `""` - *Google Sheets* handles this, so you aren't using that (unless you're looking directly at the `LOP_Game.csv` file). It's used for quotation marks so the program knows it's not the end of the text.
	- If you exported the `ko` language:
		- Copy and paste any additional entries you want to translate from `LOP_Game.csv` (the `ko` version) to your working file
		- To change the language name: Search for `GameString_Culture`
		- To change advanced graphics settings: Search for what you see in-game when the language is set to English, for example `NVIDIA DLSS Frame Generation`
		- To change the HP (Health Points) text: Search for `GameString_HealthPoint`
		- You should not translate text whose keys contain *GameString_BGM*
7. When you've finished or want to test how your work looks, place your `LOP_Game.csv` file in the same folder as `UnrealLocres.exe`
8. Open Command Prompt in that folder
9. Run the command: `UnrealLocres.exe import LOP_Game.locres LOP_Game.csv -f csv`
10. If everything went well, you should have a `LOP_Game.locres.new` file in the folder. This is your translated localization file.
11. Copy it to the `LiesofP\Content\Localization\LOP_Game\en\` folder (or the language you're overwriting), delete the original `LOP_Game.locres`, and remove the `.new` extension from the new file.

## 3. Pack Localization Files

Choose which tool you want to use for repacking: [*Repak*](#repak) (recommended) or [*Unreal Engine*](#unreal-engine)

#### Repak

1. Download [Repak](https://github.com/matyalatte/UE4-DDS-Tools/releases/latest)
	- If you chose the `.msi` version: Simply install it
	- If you chose the `.zip` version: Extract it
2. Open a **new** Command Prompt window in the folder containing your `LiesofP` folder
	- If you downloaded the `.zip` version, place `repak.exe` next to the `LiesofP` folder
3. Run the command: `repak pack LiesofP\Content Your_Translation.pak -m ../../../LiesofP/Content/ --compression Zlib`
4. You should now have a `Your_Translation.pak` file in the folder.
5. Copy it to the `Lies of P\LiesofP\Content\Paks` folder
6. **Launch the game and check if your translation works!**

#### Unreal Engine

1. Download [Unreal Engine 4.27.X](https://www.unrealengine.com/en-US/download)
2. Navigate to the `UE_4.27\Engine\Binaries\Win64` folder of your Unreal Engine installation
3. Create a file named `LiesOfP_pack.txt` (or whatever you prefer) and add this line: `"C:\your\path\to\LiesOfP\Content\*" "../../../LiesofP/Content/"`
	- Make sure to change the path to your actual Lies of P `Content` folder path
4. Open Command Prompt in that folder
5. Run the command: `UnrealPak.exe Your_Translation.pak -Create=LiesOfP_pack.txt -compress`
6. Wait 10-20 seconds. If everything went well, you should have a `Your_Translation.pak` file in the folder.
7. Copy it to the `Lies of P\LiesofP\Content\Paks` folder
8. **Launch the game and check if your translation works!**

## Special Texts
1. Join the [Lies Of P Modding Discord](https://discord.gg/UWK9XJ9xSZ) and go to the `#lies_of_p_resources` channel.
2. Find and download the latest `.usmap` file
3. Return to *FModel* and click **Directory > Settings > General**
4. Find the **Local Mapping File** field and set it to **Enabled**
5. A **Mapping File Path** field should appear; select the `.usmap` file you downloaded
6. Go back to **Archives** (where the `.pak` files are), double-click on `pakchunk0_s5-WindowsNoEditor.pak`, and navigate to `LiesofP/Content/L10N/en/UI/Texture/ImgText/`
	- Only the `en` folder works; don't choose `ko`
7. Right-click on the `ImgText` folder and select **Export Folder's Packages Raw Data (.uasset)**, then right-click again and select **Save Folder's Packages Textures**
8. Follow ***Step 9*** from the [1. Export Localization Files](#1-export-localization-files) section
9. Find the `ImgText` folder in the exported files and move it to another location
10. Modify the images as needed
11. When you've finished or want to test how your work looks, download [UE4-DDS-Tools](https://github.com/matyalatte/UE4-DDS-Tools/releases/latest) (GUI version)
12. Extract the downloaded file and open `GUI.exe`
13. Configure the settings as follows:
	- **Uasset file**: The `.uasset` file you want to convert from the `ImgText` folder
	- **Texture file**: The modified image you want to use
	- **Output folder**: The folder where you want to save the converted `.uasset` file
	- **UE version**: `4.26 ~ 4.27`
	- **No mipmaps**: Unchecked
	- **Force uncompressed**: Unchecked
	- **Skip non-texture assets**: Checked
	- **Use cubic filter**: Checked
14. Click **Inject**
15. Navigate to the output folder you selected in the previous step, and copy the newly created `.uasset` and `.uexp` files to `LiesofP\Content\L10N\en\UI\Texture\ImgText\`
	- Delete any exported `.png` files if they're still present