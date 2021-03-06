---
title: "Singapore figures"
author: "Venus Lau; Lauren Tindale"
output: 
  html_document:
    keep_md: TRUE
---



#Calculate average duration between exposure, symptom onset, hospitaization, and discharge

```r
data$days_between_inf_symp = difftime(data$date_onset_symptoms, data$presumed_infected_date, units = "days")
data$days_between_symp_hosp = difftime(data$date_hospital, data$date_onset_symptoms, units = "days")
data$days_between_hos_dis = difftime(data$date_discharge, data$date_hospital, units = "days")

mean<-data %>% select(days_between_inf_symp:days_between_hos_dis) %>% summarise_all(list(mean=mean, sd=sd), na.rm=TRUE)
```




```r
cols <- c("Cumulative hospitalized"="#440154FF", "Cumulative discharged"="#1F968BFF", "Hospitalized per day"="#39568CFF")

p<-data %>%
ggplot() +
  geom_bar(aes(x = date_hospital), fill = "#39568CFF") +
  geom_line(aes(x = dummy_hosdates, y = cumsum(dummy_value), color = "Cumulative hospitalized"), size = 1) +
  geom_line(aes(x = dummy_disdates, y = cumsum(dummy_value), color = "Cumulative discharged"), size = 1) +
  
  geom_blank(aes(color = "Hospitalized per day")) +
  xlab(label = "Date") +
  ylab(label = "Number of Cases") +
  ggtitle(label = "Singapore COVID-19 Cases") +
  theme(plot.title = element_text(hjust = 0.5, size =16)) + #centre main title
  theme(axis.text.x = element_text(angle =60, hjust = 1, size = 8),
        axis.title = element_text(size=14),
        axis.ticks.x = element_blank(), #remove x axis ticks
        axis.ticks.y = element_blank(),
        legend.text=element_text(size=12),
        legend.title = element_text(size=14)) + #remove y axis ticks
  scale_x_date(date_breaks = "day") +
  scale_y_continuous(breaks=pretty_breaks(n=20)) +
  scale_colour_manual(name = "Legend", values = cols) +
  theme(panel.background = element_rect(fill = "white"))+
  annotate(geom="text", x=as.Date("2020-02-26"), y=93, label="93", hjust="left") +
  annotate(geom="text", x=as.Date("2020-02-27"), y=62, label="62") 
  

#ggsave("final_figures/Fig1a_case_count_singapore.pdf",plot=p, device="pdf",width = 10,height = 7.4,units="in")
```


