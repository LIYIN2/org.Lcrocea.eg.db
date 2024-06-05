# org.Lcrocea.eg.db

## R package for enrichment analysis of large yellow croaker

## 1. 确保旧版R包卸载

```r
remove.packages("org.Lcrocea.eg.db")
```

## 2. 下载

### 此链接黏贴至浏览器下载,后解压缩

```

https://github.com/LIYIN2/org.Lcrocea.eg.db/releases/download/v2/org.Lcrocea.eg.db_20240602.zip
```

## 3. 安装

```r
BiocManager::install("AnnotationDbi")
library(AnnotationDbi)
install.packages("./org.Lcrocea.eg.db/", repos = NULL, type = "sources")
```

## 4. 加载

```r
library(org.Lcrocea.eg.db)
columns(org.Lcrocea.eg.db)
```

## 5. 卸载

```r
remove.packages("org.Lcrocea.eg.db")
```
