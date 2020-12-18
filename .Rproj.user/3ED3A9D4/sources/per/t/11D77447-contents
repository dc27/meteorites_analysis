# Section 1 ----
# Cleaning function
clean_data <- function(meteorite_data) {
  # check raw data is in expected form:
  desired_col_names <- c("id",
                         "name",
                         "mass (g)",
                         "fall",
                         "year",
                         "GeoLocation")
  
  assert_that(length(meteorite_data) == 6)
  assert_that(all(names(meteorite_data) == desired_col_names))
  

  # 1. rename columns according to convention
  meteorites <- meteorite_data %>% 
    rename(mass = "mass (g)",
           geolocation = "GeoLocation")

  pattern <- "[0-9., -]+"
  
  # 2. split the location column into separate columns for latitude and 
  # longitude
  meteorites_long <- meteorites %>% 
    mutate(geolocation = str_extract(geolocation, pattern)) %>%
    separate(geolocation,
             into = c("latitude", "longitude"),
             sep = ", ") %>% 
    # location columns hold only numbers
    mutate(latitude = as.numeric(latitude),
           longitude = as.numeric(longitude))
  
  # replace any missing lat/longs with 0s
  meteorites_long <- meteorites_long %>% 
    mutate(latitude =
             case_when(
               is.na(latitude) == TRUE ~ 0,
               TRUE ~ latitude
             ),
           longitude = 
             case_when(
               is.na(longitude) == TRUE ~ 0,
               TRUE ~ longitude
             ))
  
  #3. remove meteorites less than 1000g
  meteorites_long <- meteorites_long %>%
    filter(mass > 1000) 
    
  #4. order by the year of discovery
  meteorites_long <- meteorites_long %>% 
    arrange(year)
  
  # the error messages are clearer if split for each one
  # 
  meteorites_long %>% 
    verify(latitude > -90) %>% 
    verify(latitude < 90) %>% 
    verify(longitude > -180) %>% 
    verify(longitude < 180)
  
  return(meteorites_long)
  
}