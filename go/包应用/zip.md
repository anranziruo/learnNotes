### zip下载csv

#### CSV组成buffer
```
func CsvWriter(fileName string, csvDatas [][]string) (csvBuffer *bytes.Buffer, err error) {	
    csvBuffer = bytes.NewBuffer(nil)
    csvWriter := csv.NewWriter(csvBuffer)	
    if err = csvWriter.WriteAll(csvDatas); err != nil {
        return csvBuffer, err
    }   	
    return csvBuffer, nil
}
``` 
#### zip文件下载
```
func SampleDownLoadFiles() (zipBuffer *bytes.Buffer, err error) {
	zipBuffer = bytes.NewBuffer(nil)
	zipWriter := zip.NewWriter(zipBuffer)
	defer zipWriter.Close()
	for _, params := range downLoadParams {
		var downFileData [][]string
		csvBuffer, csvWriteErr := CsvWriter(params.DownloadName, downFileData)
		if csvWriteErr != nil {
			return zipBuffer, csvWriteErr
		}
		csvFile, createFileErr := zipWriter.Create(params.DownloadName)
		if createFileErr != nil {
			return zipBuffer, createFileErr
		}
		tempStr := fmt.Sprintf("\xEF\xBB\xBF%s", string(csvBuffer.Bytes())) //防止中文乱码
		_, writeErr := csvFile.Write([]byte(tempStr))
		if writeErr != nil {
			return zipBuffer, writeErr
		}
	}
	return zipBuffer, nil
}
```