# category-tree-in-category-page

Prestashop 1.6.x by default displays only subcategories in parent category page. You can add a second level of subcategories by adding this lines of code:<br>

Open /controllers/front/CategoryController.php , find the function "protected function assignSubcategories()" and add this code<br>
			$sub_sub_categories = array();
			foreach ($sub_categories as $sub_category) { 
			$sub_sub_categories[$sub_category['id_category']] = Category::getChildren($sub_category['id_category'], $this->context->language->id); 
			}
			if ($sub_sub_categories) {
            $this->context->smarty->assign(array(
                'sub_subcategories'      => $sub_sub_categories
            ));
			}
      
 in this condition "if ($sub_categories = $this->category->getSubCategories($this->context->language->id)) {" before the end.<br>
