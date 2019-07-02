## First pull out the data from NHANES. I'm including Gender in case we want to do something with it later.##
data1 <- NHANES[1:10000,c('DaysMentHlthBad','BMI','Gender')]
## Remove records where there was zero bad mental health days reported ##
library(dplyr)
data1 <- subset(data1, DaysMentHlthBad > 0)
## View a summary of your data to verify it looks right ##
summary(data1)
## Notice there are no NAs for DaysMentHlthBad because we just pulled out only records with >0 DaysMentHlthBad ##
## Now lets impute for BMI so that we don't have any NA values in BMI ##
imp<-mice(data1,m=5)
## And now lets complete the imputation and save it as impdata1 ##
impdata1<-complete(imp)
## Lets verify by pulling a summary of impdata1 ##
summary(impdata1)
## Notice no more NAs ##
## Lets run a plot and see what it looks like ##
ggplot(impdata1, aes(x = DaysMentHlthBad, y = BMI)) + geom_bin2d()
## It appears that due to some outlier BMI values the plot is hard to read ##
## Lets try using some bins on the BMI variable to see if that helps the plot ##
impdata1 <- mutate(impdata1, BMI_Group = cut(BMI, breaks = c(0, 20, 25, 30, 35, 40, 100)))
## Now lest rerun the plot using BMI_Group ##
ggplot(impdata1, aes(x = DaysMentHlthBad, y = BMI_Group)) + geom_bin2d()
## That looks better ##
## Now we just need to clean up the visual ##
