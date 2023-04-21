A bar chart was then created to analyze the total amount that the
company spent on each advertising media.

``` r
# To create a bar chart to analyze the total amount that the company spent on 
# each advertising media:
box_plot_df_long %>%
  group_by(variable) %>%
  summarize(sum = sum(value, na.rm=TRUE)) %>%
  ungroup() %>%
  ggplot(aes(reorder(factor(variable), -sum), sum)) + 
  geom_col(width = 0.4) +
  geom_text(aes(label = dollar(sum),
                vjust = -0.7, hjust = 0.5)) +
  labs(title = "Total Expense by Advertising Media",
       x = "Advertising Media",
       y = "Total Expense") +
  theme(text = element_text(size = 12.50)) +
  scale_y_continuous(labels=scales::dollar_format(), limits = c(0, 32000000), 
                   breaks = seq(0, 32000000, by = 8000000))
```

![](Sales-Quantity-Prediction-Based-on-Advertising-Media-Investment_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

The company invested most of its advertising budget in television,
followed by newspaper, and then radio.
