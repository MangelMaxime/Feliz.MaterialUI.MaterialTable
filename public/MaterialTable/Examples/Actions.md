﻿# Feliz.MaterialUI.MaterialTable - Actions

Taken from [material-table - Actions](https://material-table.com/#/docs/features/actions)

```fsharp:materialtable-actions
[<RequireQualifiedAccess>]
module Samples.Actions

open Elmish.SweetAlert
open Fable.Core.Experimental
open Fable.MaterialUI.Icons
open Feliz
open Feliz.ElmishComponents
open Feliz.MaterialUI
open Feliz.MaterialUI.MaterialTable

type private RowData =
    { name: string
      surname: string
      birthYear: int
      birthCity: int }

type private Model = { Dummy: string }

let private init () = { Dummy = "" }

type private Msg =
    | AddRow
    | SaveRow

let private update msg model =
    match msg with
    | AddRow ->
        model,
        SimpleAlert("Not in my demo you ain't!")
            .Type(AlertType.Error)
        |> SweetAlert.Run
    | SaveRow ->
        model,
        SimpleAlert("Saved!")
            .Type(AlertType.Success)
        |> SweetAlert.Run

let private view = React.functionComponent (fun (input: {| model: Model; dispatch: Msg -> unit |}) ->
    let theme = Styles.useTheme()

    Mui.materialTable [
        prop.style [ style.backgroundColor theme.palette.background.``default`` ]
        materialTable.title "Simple Action Preview"
        materialTable.columns [
            columns.column [
                column.title "Name"
                column.field<RowData> (fun rd -> nameof rd.name)
            ]
            columns.column [
                column.title "Surname"
                column.field<RowData> (fun rd -> nameof rd.surname)
            ]
            columns.column [
                column.title "Birth Year"
                column.field<RowData> (fun rd -> nameof rd.birthYear)
                column.type'.numeric
            ]
            columns.column [
                column.title "Birth Place"
                column.field<RowData> (fun rd -> nameof rd.birthCity)
                column.lookup<int,string> [ 
                    (34, "İstanbul")
                    (63, "Şanlıurfa") 
                ]
            ]
        ]
        materialTable.data [
            { name = "Mehmet"; surname = "Baran"; birthYear = 1987; birthCity = 63 }
            { name = "Zerya Betül"; surname = "Baran"; birthYear = 2017; birthCity = 34 }
        ]
        materialTable.actions [
            actions.action [
                action.icon (Mui.icon [ addIcon [] ])
                action.tooltip "Add User"
                action.isFreeAction true
                action.onClick (fun _ _ -> input.dispatch AddRow)
            ]
            actions.action<RowData> (fun rowData -> [
                action.icon (Mui.icon [ saveIcon [] ])
                action.tooltip (sprintf "Save %s" rowData.name)
                action.onClick (fun _ _ -> input.dispatch SaveRow)
            ])
        ]
        materialTable.options [
            options.headerStyle [
                style.paddingLeft (length.em 1)
            ]
        ]
    ])

let render () = React.elmishComponent("freeAction", init(), update, (fun model dispatch -> view {| model = model; dispatch = dispatch |} ))
```