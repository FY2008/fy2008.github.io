---
title: FreeRTOS STM32 实现LED闪烁，KEY任务挂起与恢复任务
date: 2021-01-04 10:14:20
tags:
- STM32
- FreeRTOS
- 编程
category: 电子技术
index_img: /img/NUCLEO-F410RB.webp
banner_img: /img/8.jpg
---

这两天跟着`哔哩哔哩`学习了下 `FreeRTOS`，这里把学习中实现的一个入门代码分享出来。

## 功能
这里实现的功能是，共有连个任务，一个 `LED` 闪烁任务、一个`KEY`按键任务，通过判断按键是否按下来执行`挂起任务`和`恢复任务`。

* 按键按下
  * 挂起LED任务 vTaskSuspend(defaultTaskHandle);
* 按键松开
  * 恢复LED任务 vTaskResume(defaultTaskHandle);

## STM32CubeMX 配置
我的开发板型号是 NUCLEO-F410RB 开发板，

gpio 配置图
![gpio配置图](/img/GPIO_Config.png)
`GPIO`配置分为`LED`的io配置和`KEY`的io配置，key配置为外部中断`上下沿`触发模式，`LED`的io配置为默认的输出模式。

key按键配置
![key按键配置](/img/key-config.png)

sys 配置
![sys配置](/img/sys-config.png)
SYS配置把`Timebase Source`配置为TIM6基本定时器，其他不变。



FreeRTOS配置
![FreeRTOS配置](/img/freeRTOS-config.png)
`FreeRTOS` 配置主要就是添加 `KEY`任务，把任务名和任务函数入口名修改下即可，其他默认不变。

clock配置
![clock配置](/img/clock-config.png)
时钟配置选择外部的8M晶振，`HCLK`选择最高 `100MHz`，其他默认不变。

## 具体代码

### freertos.c
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * File Name          : freertos.c
  * Description        : Code for freertos applications
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2021 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under Ultimate Liberty license
  * SLA0044, the "License"; You may not use this file except in compliance with
  * the License. You may obtain a copy of the License at:
  *                             www.st.com/SLA0044
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Includes ------------------------------------------------------------------*/
#include "FreeRTOS.h"
#include "task.h"
#include "main.h"
#include "cmsis_os.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "gpio.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
/* USER CODE BEGIN Variables */
extern UART_HandleTypeDef huart2;
extern KEY_Status KEYStatus;
/* USER CODE END Variables */
osThreadId defaultTaskHandle;
osThreadId SerialTaskHandle;
osThreadId keyTaskHandle;

/* Private function prototypes -----------------------------------------------*/
/* USER CODE BEGIN FunctionPrototypes */

/* USER CODE END FunctionPrototypes */

void StartDefaultTask(void const * argument);
void StartSerialTask(void const * argument);
void StartTaskKEY(void const * argument);

void MX_FREERTOS_Init(void); /* (MISRA C 2004 rule 8.1) */

/* GetIdleTaskMemory prototype (linked to static allocation support) */
void vApplicationGetIdleTaskMemory( StaticTask_t **ppxIdleTaskTCBBuffer, StackType_t **ppxIdleTaskStackBuffer, uint32_t *pulIdleTaskStackSize );

/* USER CODE BEGIN GET_IDLE_TASK_MEMORY */
static StaticTask_t xIdleTaskTCBBuffer;
static StackType_t xIdleStack[configMINIMAL_STACK_SIZE];

void vApplicationGetIdleTaskMemory( StaticTask_t **ppxIdleTaskTCBBuffer, StackType_t **ppxIdleTaskStackBuffer, uint32_t *pulIdleTaskStackSize )
{
  *ppxIdleTaskTCBBuffer = &xIdleTaskTCBBuffer;
  *ppxIdleTaskStackBuffer = &xIdleStack[0];
  *pulIdleTaskStackSize = configMINIMAL_STACK_SIZE;
  /* place for user code */
}
/* USER CODE END GET_IDLE_TASK_MEMORY */

/**
  * @brief  FreeRTOS initialization
  * @param  None
  * @retval None
  */
void MX_FREERTOS_Init(void) {
  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* USER CODE BEGIN RTOS_MUTEX */
  /* add mutexes, ... */
  /* USER CODE END RTOS_MUTEX */

  /* USER CODE BEGIN RTOS_SEMAPHORES */
  /* add semaphores, ... */
  /* USER CODE END RTOS_SEMAPHORES */

  /* USER CODE BEGIN RTOS_TIMERS */
  /* start timers, add new ones, ... */
  /* USER CODE END RTOS_TIMERS */

  /* USER CODE BEGIN RTOS_QUEUES */
  /* add queues, ... */
  /* USER CODE END RTOS_QUEUES */

  /* Create the thread(s) */
  /* definition and creation of defaultTask */
  osThreadDef(defaultTask, StartDefaultTask, osPriorityNormal, 0, 128);
  defaultTaskHandle = osThreadCreate(osThread(defaultTask), NULL);

  /* definition and creation of SerialTask */
  osThreadDef(SerialTask, StartSerialTask, osPriorityIdle, 0, 128);
  SerialTaskHandle = osThreadCreate(osThread(SerialTask), NULL);

  /* definition and creation of keyTask */
  osThreadDef(keyTask, StartTaskKEY, osPriorityIdle, 0, 128);
  keyTaskHandle = osThreadCreate(osThread(keyTask), NULL);

  /* USER CODE BEGIN RTOS_THREADS */
  /* add threads, ... */
  /* USER CODE END RTOS_THREADS */

}

/* USER CODE BEGIN Header_StartDefaultTask */
/**
  * @brief  Function implementing the defaultTask thread.
  * @param  argument: Not used
  * @retval None
  */
/* USER CODE END Header_StartDefaultTask */
void StartDefaultTask(void const * argument)
{
  /* USER CODE BEGIN StartDefaultTask */
  /* Infinite loop */
  for(;;)
  {
    HAL_GPIO_TogglePin(LD2_GPIO_Port, LD2_Pin);
    osDelay(1000);
  }
  /* USER CODE END StartDefaultTask */
}

/* USER CODE BEGIN Header_StartSerialTask */
/**
* @brief Function implementing the SerialTask thread.
* @param argument: Not used
* @retval None
*/
/* USER CODE END Header_StartSerialTask */
void StartSerialTask(void const * argument)
{
  /* USER CODE BEGIN StartSerialTask */
  /* Infinite loop */
  uint8_t hello[] = "hello,stm32\n";
  for(;;)
  {
    HAL_UART_Transmit(&huart2, hello, sizeof(hello), 100);
    osDelay(1000);
  }
  /* USER CODE END StartSerialTask */
}

/* USER CODE BEGIN Header_StartTaskKEY */
/**
* @brief Function implementing the keyTask thread.
* @param argument: Not used
* @retval None
*/
/* USER CODE END Header_StartTaskKEY */
void StartTaskKEY(void const * argument)
{
  /* USER CODE BEGIN StartTaskKEY */
  /* Infinite loop */
  for(;;)
  {
    if (KEYStatus == KEY_DOWN) {
      vTaskSuspend(defaultTaskHandle);  // 挂起 led 任务
      KEYStatus = KEY_UNDEFINED;    // 按键状态置位为 未知状态
    } else if(KEYStatus == KEY_UP) {
      vTaskResume(defaultTaskHandle);   // 恢复 led 任务
      KEYStatus = KEY_UNDEFINED;    // 按键状态置位为 未知状态
    }
    osDelay(10);
  }
  /* USER CODE END StartTaskKEY */
}

/* Private application code --------------------------------------------------*/
/* USER CODE BEGIN Application */

/* USER CODE END Application */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/

```
### gpio.h
```
/* USER CODE BEGIN Private defines */
typedef enum {
  KEY_DOWN,
  KEY_UP,
  KEY_UNDEFINED
} KEY_Status;
/* USER CODE END Private defines */
```

### gpio.c
```c
/* USER CODE BEGIN 0 */
KEY_Status KEYStatus = KEY_UNDEFINED;
/* USER CODE END 0 */

// 外部中断回调函数
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){
  if (GPIO_Pin == KEY_Pin) {
    if (HAL_GPIO_ReadPin(KEY_GPIO_Port, KEY_Pin) == GPIO_PIN_RESET) {
      // KEY 低电平下降沿触发
      KEYStatus = KEY_DOWN;

    } else if (HAL_GPIO_ReadPin(KEY_GPIO_Port, KEY_Pin) == GPIO_PIN_SET) {
      // KEY 高电平上升沿触发
      KEYStatus = KEY_UP;

    }
  }
}
```

## 展示
https://www.bilibili.com/video/BV1cp4y1x7H9

## FreeRTOS 视频教程列表

* 「物联网操作系统」- FreeRTOS开发训练营 ( https://www.bilibili.com/video/BV1864y1T7Z7 )