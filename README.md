# How to export UWP CellGrid to pdf

This sample demonstrates how to export [UWP CellGrid](https://help.syncfusion.com/uwp/cellgrid/overview) (SfCellGrid) to PDF.

`SfCellGrid` does not have built-in function to export the grid data to PDF. To export the data into excel, create a new `PDFGrid` and set the grid data to pdfGrid cells using `Value` property and save that modified pdfGrid in a `PdfDocument`.

``` csharp
//Create a new PDF document.
PdfDocument pdfDocument = new PdfDocument();
//Create the page
PdfPage pdfPage = pdfDocument.Pages.Add();
//Create the parent grid
PdfGrid parentPdfGrid = new PdfGrid();
parentPdfGrid.Columns.Add(grid.ColumnCount);
 
for (int i = 0; i < grid.RowCount; i++)
{
    //Add the rows
    PdfGridRow row1 = parentPdfGrid.Rows.Add();
    row1.Height = 50;
    for (int j = 0; j < grid.ColumnCount; j++)
    {
        var style = grid.Model[i, j];
        PdfGridCell pdfGridCell = parentPdfGrid.Rows[i].Cells[j];
        pdfGridCell.Value = style.CellValue;
        var brush = (style.Background as SolidColorBrush);
        //Export with style
        if (brush != null)
            pdfGridCell.Style.BackgroundBrush = new PdfSolidBrush(new PdfColor(System.Drawing.Color.FromArgb(brush.Color.A, brush.Color.R, brush.Color.G, brush.Color.B)));
    }
}
 
//Draw the PdfGrid.
parentPdfGrid.Draw(pdfPage, PointF.Empty);
StorageFile storageFile;
StorageFolder local = Windows.Storage.ApplicationData.Current.LocalFolder;
storageFile = await local.CreateFileAsync("Sample.pdf", CreationCollisionOption.ReplaceExisting);
//Save the document.
await pdfDocument.SaveAsync(storageFile);
Windows.System.Launcher.LaunchFileAsync(storageFile);
```