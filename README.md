# LibGuildRoster

## :triangular_flag_on_post: Description

This is a Lib for the Elder Scrolls Online "Guild Roster" list. There are numerous popular addons that inject custom columns into the Guild Roster, which causes a bit of chaos as they typically conflict with one another. LibGuildRoster hopes to act as a standardisation for addons to create these columns and to prevent UI failures. The traditional way to piece into the Roster is via hijacking the vanilla ZO process, if multiple addons are doing this at the same time, we have issues of layer placement, anchor recycling and in-general render tearing. All of the above degrades performance and obviously, the experience.

![Example Graphic](https://i.imgur.com/oQM3brJ.jpg)

Addons running through one 'channel' will introduce a level of comppadability that benefits everyone. This also brings up a few additional benefits, such as fixing the background texture of the Roster window. The Lib caculates how the texture should render, based on the relative content available for the current guild in the Scene.

![An extreme example](https://i.imgur.com/bbFJA3g.jpg)

An extreme example :rofl: of ITTDB, ATT, MM3 and a dummy addon, side by side through the Lib.

### Benefits

:heavy_check_mark: Compatibility

:heavy_check_mark: Working window background

:heavy_check_mark: Option to hide columns per guild

&nbsp;

## :triangular_flag_on_post: Example Usage

```lua

LibGuildRoster:AddColumn({
    
  key = 'MyAddon_CarrotCount',
        
  width = 80,
        
  header = {
    title = 'Carrots'
  },
        
  row = {

      align = TEXT_ALIGN_RIGHT,

      data = function( guildId, data, index )
      
        return getMembersCarrotCount(guildId, data.displayName)
          
      end,

      format = function( value )
      
        return zo_strformat("<<1>>", ZO_LocalizeDecimalNumber(tonumber(value)))..' carrots'
          
      end

  }
})
```

&nbsp;

## :triangular_flag_on_post: Options

### `key`
**Type:** _String_

**Example:** `'MyAddon_CarrotCount'`

**:warning: No spaces or special characters or the sorting/filtering will not work properly and drop performance**

&nbsp;

### `width`
**Type:** _Number_

**Example:** `80`

**Default:** `110`

**:white_flag: this is optional**

&nbsp;

### `header.title`
**Type:** _String_

**Example:** `Carrots`

&nbsp;

### `header.align`
**Type:** _GLOBAL_

**Requires:** `TEXT_ALIGN_LEFT`, `TEXT_ALIGN_RIGHT`, `TEXT_ALIGN_CENTER`

**Default:** `TEXT_ALIGN_LEFT`

**:white_flag: this is optional**

&nbsp;

### `header.tooltip`
**Type:** _STRING_

**Example:** `'A total of each member\'s carrots'`

**:white_flag: this is optional**

:question: This is for the tooltip when hovering over a column header, a string value can be displayed

&nbsp;

### `row.align`
**Type:** _GLOBAL_

**Requires:** `TEXT_ALIGN_LEFT`, `TEXT_ALIGN_RIGHT`, `TEXT_ALIGN_CENTER`

**Default:** `TEXT_ALIGN_LEFT`

**:white_flag: this is optional**

&nbsp;

### `row.data`
**Type:** _FUNCTION_

**Args:** `guildId` _Number_, `rowData` _Object_, `rowIndex` _Number_

**Example:**
```lua
...
data = function( guildId, data, index )
    
    -- Return an unformated raw value
    return MyAddon:GetCarrotCount( guildId, data.displayName )

end,
...
```
**:warning: Function must return a raw value, to design the value, see** `row.format` **below**

**:white_flag: this is optional - See** `LibGuildRoster:SetBulkData()` **for other approach**

&nbsp;

### `row.format`
**Type:** _FUNCTION_

**Args:** `rowCallValue` _String_

**Example:**
```lua
...
format = function( value )
    
    return zo_strformat("<<1>>", ZO_LocalizeDecimalNumber(tonumber(value)))..' carrots'

end,
...
```
**:warning: `value` will always be passed in as a _String_**

**:white_flag: this is optional**

&nbsp;

### `row.mouseEnabled`
**Type:** _FUNCTION_

**Args:** `guildId` _Number_, `rowData` _Object_, `rowCellValue` _( Number, String )_

**Example:**
```lua
...
mouseEnabled = function( guildId, data, value )
    
    local condition = false
    
    if value >= 1 then
        condition = true
    end
    
    return condition

end,
...
```
**:warning: Function must return a true/false value**

**:white_flag: this is optional**

&nbsp;

### `row.OnMouseEnter`
**Type:** _FUNCTION_

**Args:** `guildId` _Number_, `rowData` _Object_, `control` _Object / GUI_

**Example:**
```lua
...
OnMouseEnter = function( guildId, data, control )

    InitializeTooltip(MyAddonTooltip)
    MyAddonTooltip:SetDimensionConstraints(380,-1,440,-1)
    MyAddonTooltip:ClearAnchors()
    MyAddonTooltip:SetAnchor(BOTTOMRIGHT, control, TOPLEFT, 100, 0)
    MyAddonTooltip_GetInfo(MyAddonTooltip, data.displayName)

end,
...
```
**:white_flag: this is optional**

&nbsp;

### `row.OnMouseExit`
**Type:** _FUNCTION_

**Args:** `guildId` _Number_, `rowData` _object_, `control` _object/GUI_

**Example:**
```lua
...
OnMouseExit = function( guildId, data, control )

    ClearTooltip(MyAddonTooltip)

end,
...
```
**:white_flag: this is optional**

&nbsp;


## :triangular_flag_on_post: API Reference

### LibGuildRoster Methods

#### LibGuildRoster:AddColumn( _settings_ )

#### LibGuildRoster:Refresh()

#### LibGuildRoster:SetBulkData( _guildList_, _callback_ )

### Column Methods

#### Column:GetHeader()

#### Column:IsDisabled( _value_ )

#### Column:SetGuildFilter( _manifest_ )

#### Column:UpdateRowData( _rowDataCallback_ )
