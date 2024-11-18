# org.Lcrocea.eg.db

## R package for enrichment analysis of large yellow croaker

## 1. 确保旧版R包卸载

```r
remove.packages("org.Lcrocea.eg.db")

```

## 2. 下载

### 此链接黏贴至浏览器下载,后解压缩

```

https://github.com/LIYIN2/org.Lcrocea.eg.db/releases/download/v2/org.Lcrocea.eg.db_20240615.zip

```

## 3. 安装

```r
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

## 6. 应用

### 6.1. GO富集分析

```r
library(clusterProfiler)
gene <- read.csv(argv$deg)
rich <- enrichGO(gene = gene$id,                              
                 keyType = "GID",                                   
                 OrgDb = org.Lcrocea.eg.db,                          
                 ont = "all",                                        
                 pvalueCutoff = 0.05,                             
                 qvalueCutoff = 0.2,                                 
                 pAdjustMethod = "BH",
                 readable = FALSE) 
```

### 6.2 KEGG富集分析

```r
library(tidyverse)
library(clusterProfiler)

{pathway2gene <- AnnotationDbi::select(org.Lcrocea.eg.db,
                                       keys = keys(org.Lcrocea.eg.db),
                                       columns = c("Pathway","Ko")) %>%
    na.omit() %>% 
    dplyr::select(Pathway, GID)
  if (!file.exists("../kegg_info.RData")) {
    download.file("https://github.com/LIYIN2/org.Lcrocea.eg.db/raw/main/kegg_info.RData",
    destfile = "../kegg_info.RData")}
  load("../kegg_info.RData")
  gene2des <-left_join(pathway2gene,pathway2name) %>% unique()}

rich <- enricher(gene = gene$id,
                 TERM2GENE = gene2des[c("Pathway","GID")],
                 TERM2NAME = gene2des[c("Pathway","Name")],
                 pvalueCutoff = 0.05,
                 pAdjustMethod = 'BH',
                 qvalueCutoff = 02,
                 maxGSSize = 500)
```

### 6.3 GSEA富集分析

```r
### 基因集
{pathway2gene <- AnnotationDbi::select(org.Lcrocea.eg.db,
                                       keys = keys(org.Lcrocea.eg.db),
                                       columns = c("Pathway","Ko")) %>%
    na.omit() %>% 
    dplyr::select(Pathway, GID)
if (!file.exists("../kegg_info.RData")) {
    download.file("https://github.com/LIYIN2/org.Lcrocea.eg.db/raw/main/kegg_info.RData",
                  destfile = "../kegg_info.RData")}
load("../kegg_info.RData")
gene2des <-left_join(pathway2gene,pathway2name) %>% unique()
geneSet <-gene2des  %>% 
    select(Description="Name",
           geneID="GID") }
### 背景基因列表
exp <- read.csv(argv$exp) %>%   dplyr::select(c("id","log2FoldChange"))

geneList <- exp$log2FoldChange
names(geneList) <- exp$id
geneList <- sort(geneList, decreasing = T) 

GSEA_enrichment <- GSEA(geneList, 
                        TERM2GENE = geneSet,
                        pvalueCutoff = 0.05,
                        minGSSize = 10, 
                        maxGSSize = 500, 
                        eps = 0,
                        pAdjustMethod = "BH") 
```

### 6.4 获取基因sympol和功能信息

```r
gene_info <- AnnotationDbi::select(org.Lcrocea.eg.db,
                     keys = keys(org.Lcrocea.eg.db),
                     columns = c("GID","gene_sympol","description"))
```

### 6.5 获取pfam结构域信息

```r
gene_info <- AnnotationDbi::select(org.Lcrocea.eg.db,
                                   keys = keys(org.Lcrocea.eg.db),
                                   columns = c("GID","gene_sympol","pfam_sympol"))
```
### 7.0 CroceaEnrich包使用
```r
library(devtools)
devtools::install_github("LIYIN2/CroceaEnrich")
CroceaEnrich(file, org.Lcrocea.eg.db, prefix = "CroceaEnrich_Result")
```
