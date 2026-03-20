# BIO-ORACLE



COMPOSABLE APP:

package com.biooracle.app

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.material3.Button
import androidx.compose.material3.Text
import androidx.compose.material3.MaterialTheme
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.unit.sp
import androidx.compose.ui.unit.dp
import androidx.compose.ui.tooling.preview.Preview

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            MenuScreen()
        }
    }
}

@Composable
fun MenuScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .background(Color(0xFF161B22)),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {

        Text(
            text = "BIO ORACLE",
            color = Color.White,
            fontSize = 28.sp,
            modifier = Modifier.padding(bottom = 32.dp)
        )

        Button(
            onClick = { /* luego irá cámara */ },
            modifier = Modifier
                .fillMaxWidth(0.7f)
                .padding(8.dp)
        ) {
            Text("CAPTURAR")
        }

        Button(
            onClick = { },
            modifier = Modifier
                .fillMaxWidth(0.7f)
                .padding(8.dp)
        ) {
            Text("MAPA")
        }

        Button(
            onClick = { },
            modifier = Modifier
                .fillMaxWidth(0.7f)
                .padding(8.dp)
        ) {
            Text("APORTES DESTACADOS")
        }

        Button(
            onClick = { },
            modifier = Modifier
                .fillMaxWidth(0.7f)
                .padding(8.dp)
        ) {
            Text("SALIR")
        }
    }
}

@Preview(showBackground = true)
@Composable
fun PreviewMenu() {
    MenuScreen()
}
