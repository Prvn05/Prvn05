// MainActivity.kt
package com.example.mytools

import android.content.Intent
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import android.widget.Button

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val viewProductsButton: Button = findViewById(R.id.view_products_button)
        viewProductsButton.setOnClickListener {
            startActivity(Intent(this, ProductListActivity::class.java))
        }

        val supportButton: Button = findViewById(R.id.support_button)
        supportButton.setOnClickListener {
            startActivity(Intent(this, SupportActivity::class.java))
        }
    }
}
// ProductListActivity.kt
package com.example.mytools

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import android.widget.ListView
import okhttp3.*
import org.json.JSONArray
import java.io.IOException

class ProductListActivity : AppCompatActivity() {
    private lateinit var listView: ListView
    private val client = OkHttpClient()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_product_list)
        listView = findViewById(R.id.product_list_view)

        fetchProducts()
    }

    private fun fetchProducts() {
        val request = Request.Builder()
            .url("http://10.0.2.2:3000/products") // Replace with your backend URL
            .build()

        client.newCall(request).enqueue(object : Callback {
            override fun onFailure(call: Call, e: IOException) {
                // Handle failure
            }

            override fun onResponse(call: Call, response: Response) {
                val json = response.body?.string()
                runOnUiThread {
                    // Parse and update product list view (ListAdapter logic here)
                }
            }
        })
    }
}
// LoginActivity.kt
package com.example.mytools

import android.content.Intent
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import android.widget.Button
import android.widget.EditText
import okhttp3.*

class LoginActivity : AppCompatActivity() {
    private val client = OkHttpClient()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        val emailEditText: EditText = findViewById(R.id.email)
        val passwordEditText: EditText = findViewById(R.id.password)
        val loginButton: Button = findViewById(R.id.login_button)

        loginButton.setOnClickListener {
            val email = emailEditText.text.toString()
            val password = passwordEditText.text.toString()
            loginUser(email, password)
        }
    }

    private fun loginUser(email: String, password: String) {
        val requestBody = FormBody.Builder()
            .add("email", email)
            .add("password", password)
            .build()

        val request = Request.Builder()
            .url("http://10.0.2.2:3000/login") // Replace with your backend URL
            .post(requestBody)
            .build()

        client.newCall(request).enqueue(object : Callback {
            override fun onFailure(call: Call, e: IOException) {
                // Handle failure
            }

            override fun onResponse(call: Call, response: Response) {
                val json = response.body?.string()
                runOnUiThread {
                    // Handle login success and store token
                }
            }
        })
    }
}
