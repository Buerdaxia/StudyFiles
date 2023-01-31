# z-index遇到的问题

**并不是z-index设置的越高，它的层级就越高，以下这些情况你需要考虑到**



​	1.未设置position的（position为static）的情况下，z-index不起作用（z-index在relative/absolute/fixed的情况下才有效）

​	2.设置position不为static的情况下



​	同一级的元素设置了z-index：z-index越大，层级越高，越靠前未设置z-index：DOM树中越靠后的元素层级越高，越靠前

​	

​	**不同级的元素父元素设置了z-index**：**子元素始终在父元素上边***，即使子元素的z-index小于父元素的z-index**父元素没有设置z-index**：**子元素设置负的z-index可位于父元素的下边**，其余全位于父元素的上边



所以说，子元素要放到父元素下面，父元素一定是不能设置z-index值的