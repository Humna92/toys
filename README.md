# toys
#clean up brand names
toys <- data.frame(refine_original %>% lapply(tolower))
toys$company[toys$company %in% c("philips", "fillips", "phlips", "phillps", "phllips")] <- "phillips"
toys$company[toys$company %in% c("akz0", "ak zo")] <- "akzo"
toys$company[toys$company %in% c("unilver")] <- "unilever"
#separate product code and number
toys2 <- (toys %>% separate(Product.code...number, c("product_code", "number"), sep="-"))
#add product categories
product_category <- c("Smartphone", "Smartphone", "Laptop", "Laptop", "Laptop", "Smartphone", "TV", "TV", "Laptop", "Smartphone", "Tablet", "Tablet", "Laptop", "Smartphone", "TV", "TV", "Laptop", "TV", "TV", "Laptop", "Smartphone", "Laptop", "Tablet", "Tablet", "Tablet")
toys3 <- (toys2 %>% cbind(product_category))
#add full address for geocoding
toys4 <- (unite(toys3, geocode, address, city, country, sep = ", "))
#create dummies
toys4 <- within(toys4,company_phillips<-ifelse(company=="phillips",1,0)) 
toys4 <- within(toys4,company_akzo<-ifelse(company=="akzo",1,0))
toys4 <- within(toys4,company_unilever<-ifelse(company=="unilever",1,0))
toys4 <- within(toys4,company_van_houten<-ifelse(company=="van houten",1,0))
toys4 <- within(toys4,product_smartphone <- ifelse(product_category=="Smartphone",1,0))
toys4 <- within(toys4,product_tv <- ifelse(product_category=="TV",1,0))
toys4 <- within(toys4,product_laptop <- ifelse(product_category=="Laptop",1,0))
toys4 <- within(toys4,product_tablet <- ifelse(product_category=="Tablet",1,0))
