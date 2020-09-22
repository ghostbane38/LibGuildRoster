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
