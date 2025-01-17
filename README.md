# VBA-challenge


Background
You are well on your way to becoming a programmer and Excel expert! In this homework assignment, you will use VBA scripting to analyze generated stock market data.

Before You Begin
Create a new repository for this project called VBA-challenge. Do not add this assignment to an existing repository.

Inside the new repository that you just created, add any VBA files that you use for this assignment. These will be the main scripts to run for each analysis.

The below is the coding used for the VBA


Sub stock_screener():
    For Each ws In Worksheets:
        Dim ticker As String
        Dim open_price, close_price, yearly_change, percent_change As Double
        Dim summary_row As Integer
        Dim last_row As Long
        Dim total_volume As Variant
        
        last_row = ws.Cells(Rows.Count, 1).End(xlUp).Row
        
        total_volume = 0
        summary_row = 1
        
        ws.Range("I" & summary_row) = "Ticker"
        ws.Range("J" & summary_row) = "Yearly Change"
        ws.Range("K" & summary_row) = "Percent Change"
        ws.Range("L" & summary_row) = "Total Stock Volume"
        
        summary_row = 2
        open_price = ws.Cells(2, 3).Value
        
        'Calculates each ticker symbol's change and volume
        For i = 2 To last_row
            ticker = ws.Cells(i, 1).Value
            
            total_volume = total_volume + ws.Cells(i, 7).Value
            
            If (ticker <> ws.Cells(i + 1, 1).Value) Then
                
                close_price = ws.Cells(i, 6).Value
                yearly_change = close_price - open_price
                percent_change = yearly_change / open_price
                
                ws.Range("I" & summary_row) = ticker
                ws.Range("J" & summary_row) = yearly_change
                ws.Range("K" & summary_row) = FormatPercent(percent_change)
                ws.Range("L" & summary_row) = total_volume
                
                If (yearly_change <= 0) Then
                    ws.Cells(summary_row, 10).Interior.ColorIndex = 3
                Else: ws.Range("J" & summary_row).Interior.ColorIndex = 4
                End If
            
                open_price = ws.Cells(i + 1, 3).Value
                total_volume = 0
                summary_row = summary_row + 1
    
            End If
     
        Next i
        
        'Extreme values
        ws.Range("O1") = "Ticker"
        ws.Range("P1") = "Value"
        ws.Range("N2") = "Greatest % Increase"
        ws.Range("N3") = "Greatest % Decrease"
        ws.Range("N4") = "Greatest Total Volume"
        

        'Calculates ticker symbol with best performance
        Dim max_ticker As String
        Dim max_value As Double
        max_value = ws.Cells(2, 11).Value
        For i = 3 To summary_row
            If (ws.Cells(i, 11).Value > max_value) Then
            max_ticker = ws.Cells(i, 9).Value
            max_value = ws.Cells(i, 11).Value
            End If
        Next i
        ws.Range("O2").Value = max_ticker
        ws.Range("P2").Value = FormatPercent(max_value)
        
        'Calculates ticker symbol with worst performance
        Dim min_ticker As String
        Dim min_value As Double
        min_value = ws.Cells(2, 11).Value
        For i = 3 To summary_row
            If (ws.Cells(i, 11).Value < min_value) Then
            min_ticker = ws.Cells(i, 9).Value
            min_value = ws.Cells(i, 11).Value
            End If
        Next i
        ws.Range("O3").Value = min_ticker
        ws.Range("P3").Value = FormatPercent(min_value)
        
        'Calculates maximum volume per ticker symbol
        Dim max_volume_ticker As String
        Dim max_volume_value As Double
        max_volume_value = ws.Cells(2, 12).Value
        For i = 3 To summary_row
            If (ws.Cells(i, 12).Value > max_volume_value) Then
            max_volume_ticker = ws.Cells(i, 9).Value
            max_volume_value = ws.Cells(i, 12).Value
            End If
        Next i
        ws.Range("O4").Value = max_volume_ticker
        ws.Range("P4").Value = max_volume_value
        
        ws.Range("A:Q").Columns.AutoFit
    Next

End Sub
