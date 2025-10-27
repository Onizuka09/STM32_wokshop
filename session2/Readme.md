# Session2 

## GPIO Output config 
- We configured the green user led as output: `PD12`

```c 
// private defines 
#define LED_GREEN GPIO_PIN_12
#define LED_GREEN_PORT GPIOD
// functions 
void MX_GPIO_Init(void){
	// enable clock for GPIOD
	__HAL_RCC_GPIOD_CLK_ENABLE() ;
	// pin configuration
	GPIO_InitTypeDef gpiod;
	gpiod.Pin = LED_GREEN ;
	gpiod.Mode = GPIO_MODE_OUTPUT_PP ;
	gpiod.Pull = GPIO_NOPULL ;
	gpiod.Speed = GPIO_SPEED_FREQ_MEDIUM ;
	// save the configuration
	HAL_GPIO_Init(LED_GREEN_PORT, &gpiod);
}

// main 
int main(){

    HAL_Init(); 
    SystemClock_Config(); 

    // our logic 
    MX_GPIO_Init(); 
    while(1){
      HAL_GPIO_TogglePin(LED_GREEN_PORT, LED_GREEN);
	  HAL_Delay(1000); // wait for 1 second
    }
    return 0 
}
```
## GPIO Input config 
- We configured the user btn (blue) to be intput: `PA0`: 
```c 
// private defines 
#define BTN GPIO_PIN_0
#define BTN_PORT GPIOA
// function
void MX_GPIO_Init(void){
	// enable clock for GPIOA
	__HAL_RCC_GPIOA_CLK_ENABLE() ;
	// pin configuration
	GPIO_InitTypeDef gpioa;
	gpioa.Pin = BTN;
	gpioa.Mode = GPIO_MODE_INPUT;
	// here we dont need to configure the BTN as pullup  or pulldown because its already configured in hardware as pulldown
	gpioa.Pull = GPIO_PULLDOWN ; //   shoud be this GPIO_NOPULL , we set it as pull down just for clarification
	gpioa.Speed = GPIO_SPEED_FREQ_MEDIUM ;
	// save the configuration
	HAL_GPIO_Init(BTN_PORT, &gpioa);
}

// main 
int main(){

    HAL_Init(); 
    SystemClock_Config(); 

    // our logic 
    MX_GPIO_Init(); 
    while(1){
        // BTN press => we will read 0, because the btn is set as pulldown
        uint8_t btn = HAL_GPIO_ReadPin(BTN_PORT, BTN);
        if (btn == GPIO_PIN_SET){
            HAL_GPIO_TogglePin(LED_GREEN_PORT, LED_GREEN);
        }
    }
    return 0 
}

